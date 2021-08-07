LIB = tabix pcre
all: ${LIB}

tabix: tabix-0.2.4.tar.bz2
	tar jvxf tabix-0.2.4.tar.bz2
	(cd tabix; make; ar rcs libtabix.a *.o)

pcre: pcre-8.21.tar.gz
	-mv pcre-8.21.tar.gz* pcre-8.21.tar.gz
	tar zvxf pcre-8.21.tar.gz
	ln -s -f pcre-8.21 pcre
	-(DIR=`pwd`; cd pcre-8.21; ./configure --prefix="$${DIR}"/pcre; make -j; make install; ar rcs libpcre.a)
clean:
	rm -rf tabix-0.2.4
	rm -rf pcre-8.21
	rm -f pcre tabix
