---
layout: post
title: "Mac OSX Binary Protection"
date: 2014-12-29 17:49:19 +0800
description: ""
category: iOS Security
tags: [iOS CodeSignature]
---

### Get code signature

	codesign -l Clover
	Executable=/Users/gavingu/Desktop/Clover
	Identifier=com.tencent.clover
	Format=Mach-O thin (armv7)
	CodeDirectory v=20200 size=32782 flags=0x0(none) hashes=1630+5 location=embedded
	Hash type=sha1 size=20
	CDHash=bf502aa97e8d9bda3cd96d0cc4310afbeea1c157
	Signature size=3487
	Authority=Apple iPhone OS Application Signing
	Authority=Apple iPhone Certification Authority
	Authority=Apple Root CA
	Info.plist=not bound
	TeamIdentifier=9TV4ZYSS4J
	Sealed Resources=none
	Internal requirements count=1 size=100
	
#### list segment by otool

	otool -l Clover
	... ...
	Load command 1
    cmd LC_SEGMENT
    cmdsize 736
    segname __TEXT
    vmaddr 0x00004000
    vmsize 0x00570000
    fileoff 0
    filesize 5701632
    maxprot 0x00000005
    initprot 0x00000005
    nsects 10
    flags 0x0
    Section
    sectname __text
    segname __TEXT
      addr 0x0000aa40
      size 0x004d2670
    offset 27200
     align 2^4 (16)
    reloff 0
    nreloc 0
     flags 0x80000400
    reserved1 0
    reserved2 0
	... ...
	Load command 11
          cmd LC_ENCRYPTION_INFO
      cmdsize 20
    cryptoff  16384
    cryptsize 5685248
    cryptid   1
    ... ...
    
    
#### lipo
	otool -f test
	
	lipo -thin armv7 test -output test.armv7
	
	otool -l Clover|grep CRYPT -A 4
	
	cmd LC_ENCRYPTION_INFO
      cmdsize 20
    cryptoff  16384
    cryptsize 5685248
    cryptid   1
    
加密代码偏移 cryptoff = 16384(0x4000)

	otool -f DoubanRadio 
	
	Fat headers
	fat_magic 0xcafebabe
	nfat_arch 2
	architecture 0
    cputype 12
    cpusubtype 9
    capabilities 0x0
    offset 16384
    size 8065120
    align 2^14 (16384)
	architecture 1
    cputype 12
    cpusubtype 11
    capabilities 0x0
    offset 8093696
    size 8048672
    align 2^14 (16384)
    
    lipo -thin armv7 DoubanRadio -output DoubanRadio.armv7    
    
    otool -l DoubanRadio.armv7|grep CRYPT -A 4 
    
    cmd LC_ENCRYPTION_INFO
      cmdsize 20
    cryptoff  16384
    cryptsize 3948544
    cryptid   1
    
cryptoff = 16384(0x4000) cryptsize = 3948544(0x3c4000)

	otool -l DoubanRadio.armv7|grep _TEXT -A 3 -B 1 |head -12
	
	  cmdsize 736
  		segname __TEXT
   		vmaddr 0x00004000
   		vmsize 0x003c8000
  		fileoff 0
		--
		--
  		sectname __text
   		segname __TEXT
      	addr 0x0000b840
      	size 0x003100e8
    	offset 30784
    	
 _TEXT segment vmaddr=0x00004000 vmsize=0x003c8000
 _text section entry = 0x0000b840
 
#### gdb dump memory

(1) breakpoint
 
	rb doModInitFunctions
	b *0x0000b840
	
(2) dump memory

	startaddr = 0x4000 + 0x4000 = 0x8000
	endaddr = 0x8000 + 0x3c4000 = 0x3cc000
	
	dump memory test.bin 0x8000 0x3cc000
	
#### combine file

	otool -f DoubanRadio
	Fat headers
	fat_magic 0xcafebabe
	nfat_arch 2
	architecture 0
    cputype 12
    cpusubtype 9
    capabilities 0x0
    offset 16384
    size 8065120
    align 2^14 (16384)
	architecture 1
    cputype 12
    cpusubtype 11
    capabilities 0x0
    offset 8093696
    size 8048672
    align 2^14 (16384)
    
    archoff = 16384(0x4000) size = 8065120(0x60107b00)
    
(1) before archoff 16384(0x4000)

(2) archoff + 0x4000 = 0x8000

(3) 0x8000 size 0x3c4000         encrypt

(4)	archsize - cryptoff - cryptsize = 0x60107b00 - 0x4000 - 0x3c4000


	dd bs=1 conv=notrunc if=douban.bin of douban.dec skip=0 seek=0x8000 count = 0x3c4000
    	 
	 