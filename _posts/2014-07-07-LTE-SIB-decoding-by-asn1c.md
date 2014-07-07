---
date: 2014-07-07 17:23:00
layout: post
title: How to decode LTE RRC/SIB/ASN1 messages by asn1c
thread: 166
categories: lte
tags:  asn1c ASN1 LTE SIB decoding RRC
---

-----------------------------------------------------------------------------------

This method's main reference is: <http://blog.csdn.net/peng_yw/article/details/22437251>

This doc is used to explain how *parse_SIB()* of *Matlab/LTE_DL_receiver.m* works ( .m file is in <https://github.com/JiaoXianjun/LTE-Cell-Scanner> , which is an open source OpenCL accelerated TDD/FDD LTE Cell Scanner -- from A/D samples to RRC messages (PDSCH) out for 20MHz LTE. )

-------------------------------------------------------------------------------------

# 1. generate LTE RRC spec's ASN1 description

download 36.331 spec from <http://www.3gpp.org/ftp/Specs/archive/36_series/36.331/>

unzip: *36331-ac0.zip* --> *36331-ac0.doc*

save *36331-ac0.doc* as *36331-ac0.txt*.

get lameditor from <http://sourceforge.net/projects/lameditor/>

compile and install lameditor.

		cd lameditor-1.0/src/getasn/

copy 36331-ac0.txt here, and 

		./getasn 36331-ac0.txt

now we have *36331-ac0.asn*

# 2. generate ASN1 decoding program for LTE RRC message

get asn1c from <https://github.com/vlm/asn1c>

compile and install *asn1c*.

		cd asn1c/examples/

		mkdir sample.source.LTERRC

		cd sample.source.LTERRC

copy *36331-ac0.asn* here, and

		asn1c  -S /usr/local/share/asn1c -fcompound-names -fskeletons-copy -gen-PER -pdu=auto 36331-ac0.asn

## 2.1 modify converter-sample.c:

add 

		#define PDU BCCH_DL_SCH_Message

		#define ASN_PDU_COLLECTION

after 

		#include <asn_internal.h>

## 2.2 modify per_opentype.c:

add 

		padding = padding % 8;

after 

		ASN_DEBUG("Too large padding %d in open type", (int)padding);

and comment out following:

		_ASN_DECODE_FAILED;

## 2.3 compile the decoding program

		make -f Makefile.am.sample

here we get LTE RRC ASN1 decoding program: *progname*

# 3. usage of LTE RRC ASN1 decoding program: *progname*

		./progname recv_bits.per -p BCCH-DL-SCH-Message

where *recv_bits.per* is a binary file which contains received RRC message bits (can be gotten by HACKRF and <https://github.com/JiaoXianjun/LTE-Cell-Scanner> ) . "-p" specify message type. For example, SIB is carried on PDSCH, and its type is *BCCH-DL-SCH-Message*.


# 4. Some SIBs I decoded

## 4.1 [2360MHz at Beijing, China](/_resource/f2360_s19.2_bw20_1s_hackrf_SIB.txt)
## 4.2 [2585MHz at Beijing, China](/_resource/f2585_s19.2_bw20_1s_hackrf_SIB.txt)
## 4.3 [2585MHz at Beijing-1, China](/_resource/f2585_s19.2_bw20_1s_hackrf_1_SIB.txt)
