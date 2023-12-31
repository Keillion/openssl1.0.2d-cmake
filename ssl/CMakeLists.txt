# Based on original work by David Manura
# Copyright (C) 2007-2012 LuaDist.
# Copyright (C) 2013 Brian Sidebotham

# Redistribution and use of this file is allowed according to the terms of the
# MIT license.
#
# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in all
# copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
# SOFTWARE.

if( BUILD_SHARED_LIBS )
  add_definitions( -DOPENSSL_BUILD_SHLIBSSL )
endif()

set( LIBSRC
  s2_meth.c   s2_srvr.c s2_clnt.c  s2_lib.c  s2_enc.c s2_pkt.c 
	s3_meth.c   s3_srvr.c s3_clnt.c  s3_lib.c  s3_enc.c s3_pkt.c s3_both.c s3_cbc.c 
	s23_meth.c s23_srvr.c s23_clnt.c s23_lib.c          s23_pkt.c 
	t1_meth.c   t1_srvr.c t1_clnt.c  t1_lib.c  t1_enc.c t1_ext.c 
	d1_meth.c   d1_srvr.c d1_clnt.c  d1_lib.c  d1_pkt.c 
	d1_both.c d1_srtp.c 
	ssl_lib.c ssl_err2.c ssl_cert.c ssl_sess.c 
	ssl_ciph.c ssl_stat.c ssl_rsa.c 
	ssl_asn1.c ssl_txt.c ssl_algs.c ssl_conf.c 
	bio_ssl.c ssl_err.c kssl.c t1_reneg.c tls_srp.c t1_trce.c ssl_utst.c
  )

include_directories( BEFORE SYSTEM
  ${PROJECT_BINARY_DIR}/include
  ${PROJECT_BINARY_DIR}/include/crypto
  ${PROJECT_SOURCE_DIR}/ # e_os.h
  ${PROJECT_SOURCE_DIR}/include
)

add_library( ssl ${LIBSRC} )

target_include_directories( ssl PUBLIC
  ${PROJECT_BINARY_DIR}/include
  ${PROJECT_BINARY_DIR}/include/crypto
  ${PROJECT_SOURCE_DIR}/ # e_os.h
  ${PROJECT_SOURCE_DIR}/include
)

target_link_libraries( ssl crypto )

set_target_properties( ssl PROPERTIES
  VERSION "${LIB_VERSION}" SOVERSION "${LIB_SOVERSION}" )

install( TARGETS ssl
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib )
