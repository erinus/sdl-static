.PHONY: clean print zlib jpeg ogg vorbis freetype sdl sdl_ttf sdl_image sdl_mixer always

OS := $(OSTYPE)
OS := $(OS:darwin%=darwin)
OS := $(OS:linux%=linux)
OS := $(OS:Linux%=linux)
OS := $(OS:Windows%=windows)

SS_LIBEXT := .a
SS_OBJEXT := .o
SS_CC := gcc -O2
SS_OBJOUT := -o
EMPTY := 
SPACE :=  $(EMPTY) $(EMPTY)
SS_CREATELIB := ar -cr$(SPACE)
SDL_INC := ../include/SDL12
SDL := SDL-1.2.13
SS_CREATEDLL := gcc -dynamic -fPIC -o 

ifeq ("$(OS)","darwin")
  SS_MAC := 1
  export SS_MAC
  SS_CONFIG := mac
  LIB_DIR := ../lib/Mac
  JCONFIG := jconfig.mac
  SS_DLLEXT := .dylib
endif

ifeq ("$(OS)","iphoneos")
  SS_IPHONEOS := 1
  SS_LIBEXT := .iphoneos.a
  export SS_IPHONEOS
  export SS_IPHONE
  JCONFIG := jconfig.mac
  SS_CONFIG := iphoneos
  SS_IPHONE := 1
  LIB_DIR := ../lib/iPhone
  SDL_INC := ../include/SDL13
  SDL := SDL-1.3.0-5441
  IPHONE_VER := 2.2.1
  SS_CC := /Developer/Platforms/iPhoneOS.platform/Developer/usr/bin/gcc-4.0  \
      -DIPHONE=1 -D__IPHONEOS__=1 -O2 \
      -I../../include/SDL13 -DSDL13 \
      -arch armv6 -fmessage-length=0 -pipe -Wno-trigraphs -fpascal-strings \
      -fasm-blocks -Wreturn-type -Wunused-variable  \
       -I/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS$(IPHONE_VER).sdk/usr/include/ \
      -isysroot /Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS$(IPHONE_VER).sdk \

endif

ifeq ("$(OS)","iphonesim")
  SS_IPHONESIM := 1
  SS_LIBEXT := .iphonesim.a
  export SS_IPHONESIM
  export SS_IPHONE
  JCONFIG := jconfig.mac
  SS_CONFIG := iphonesim
  LIB_DIR := ../lib/iPhone
  SDL_INC := ../include/SDL13
  SDL := SDL-1.3.0-5441
  SS_IPHONE := 1
  IPHONE_VER := 2.2.1
  SS_CC := /Developer/Platforms/iPhoneSimulator.platform/Developer/usr/bin/gcc-4.0  \
      -DIPHONE=1 -D__IPHONESIM__=1 -O2 \
      -I../../include/SDL13 -DSDL13 \
      -arch i386 -fmessage-length=0 -pipe -Wno-trigraphs -fpascal-strings \
      -fasm-blocks -Wreturn-type -Wunused-variable  \
       -I/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneOS$(IPHONE_VER).sdk/usr/include/ \
      -isysroot /Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator$(IPHONE_VER).sdk \

endif






ifeq ("$(OS)","linux")
  SS_CONFIG := linux
  SS_LINUX := 1
  export SS_LINUX
  LIB_DIR := ../lib/Linux
  JCONFIG := jconfig.linux
  SS_DLLEXT := .dso
endif


ifeq ("$(OS)","windows")
  SS_WINDOWS := 1
  export SS_WINDOWS
  SS_CONFIG := windows
  LIB_DIR := ../lib/Windows
  SS_LIBEXT := .lib
  JCONFIG := jconfig.vc
  SS_CC := cl.exe -nologo -MT -Zi -O2 -DWIN32
  SS_OBJEXT := .obj
  SS_OBJOUT := -Fo
  SS_CREATELIB := lib -out:
  SS_DLLEXT := .dll
endif


export SS_CONFIG
export SS_CC
export SS_OBJOUT
export SS_OBJEXT
export SS_CREATELIB
export SS_LIBEXT
export SS_DLLEXT
export SS_CREATEDLL
export SDL_INC

all: print 
all: zlib 
all: png 
all: jpeg 
all: ogg 
all: vorbis 
all: freetype 
ifndef SS_IPHONE
all: smpeg 
endif
all: sdl 
all: sdl_ttf 
all: sdl_image 
all: sdl_mixer

print:
	@echo "OS: " $(OSTYPE)

# --- ZLIB ------------------------
ZLIB := zlib-1.2.3

zlib:$(ZLIB)

$(ZLIB):
	tar xvzf tars/$(ZLIB).tgz
ifdef SS_WINDOWS
	cp patches/zlib/Makefile.msc $(ZLIB)/win32
	cd $(ZLIB); unset MAKEFLAGS; nmake -f win32/Makefile.msc
	cp $(ZLIB)/zlib$(SS_LIBEXT) $(LIB_DIR)/libz$(SS_LIBEXT)
else
	cp configs/zlib-makefile $(ZLIB)/Makefile
	cd $(ZLIB); make
	cp $(ZLIB)/libz$(SS_LIBEXT) $(LIB_DIR)
endif
	cp $(ZLIB)/zlib.h $(ZLIB)/zconf.h ../include

clean::
	rm -rf $(ZLIB)

# --- PNG ------------------------

PNG := libpng-1.2.24

png:$(PNG)

$(PNG):
	tar xvzf tars/$(PNG).tgz
ifdef SS_WINDOWS
	cp configs/png-nmake-win32 $(PNG)/makefile
	cd $(PNG); unset MAKEFLAGS; nmake -f makefile
else
	cp configs/png-nmake-gcc $(PNG)/makefile
	cd $(PNG); make -f makefile
endif
	cp $(PNG)/libpng$(SS_LIBEXT) $(LIB_DIR)
	cp $(PNG)/png.h $(PNG)/pngconf.h ../include

clean::
	rm -rf $(PNG)

# --- JPEG ------------------------

JPEG := jpeg-6b

jpeg:$(JPEG)

$(JPEG):
	tar xvzf tars/$(JPEG).tgz
	cp configs/jconfig.h configs/jconfig.mac configs/jconfig.iphoneos  configs/jconfig.vc $(JPEG)
ifdef SS_WINDOWS
	cd $(JPEG); unset MAKEFLAGS; nmake -f makefile.vc nodebug=1
else
	cp configs/jpeg-makefile $(JPEG)/Makefile
	cd $(JPEG); make -f Makefile nodebug=1
endif
	cp configs/jconfig.h configs/jconfig.mac configs/jconfig.iphoneos  configs/jconfig.vc ../include
	cp $(JPEG)/jpeglib.h  $(JPEG)/jmorecfg.h ../include
	cp $(JPEG)/libjpeg$(SS_LIBEXT) $(LIB_DIR)

clean::
	rm -rf $(JPEG)

# --- OGG ------------------------

OGG := libogg-1.1.3

ogg:$(OGG)

$(OGG):
	tar xvzf tars/$(OGG).tgz
	cp configs/ogg-config_types.h $(OGG)/include/ogg/config_types.h
	cp configs/ogg-makefile $(OGG)/Makefile
	cd $(OGG); unset MAKEFLAGS; make
	cp $(OGG)/libogg$(SS_LIBEXT) $(LIB_DIR)
	mkdir -p ../include/ogg
	cp $(OGG)/include/ogg/ogg.h ../include/ogg
	cp configs/ogg-config_types.h ../include/ogg/config_types.h
	cp $(OGG)/include/ogg/os_types.h ../include/ogg

clean::
	rm -rf $(OGG)


# --- VORBIS ------------------------

VORBIS := libvorbis-1.2.0

vorbis:$(VORBIS)

$(VORBIS):
	tar xvzf tars/$(VORBIS).tgz
	cp configs/vorbis-makefile $(VORBIS)/Makefile
	cd $(VORBIS); make
	cp $(VORBIS)/libvorbis$(SS_LIBEXT) $(LIB_DIR)
	mkdir -p ../include/vorbis
	cp $(VORBIS)/include/vorbis/vorbisfile.h ../include/vorbis
	cp $(VORBIS)/include/vorbis/codec.h ../include/vorbis
	cp $(VORBIS)/include/vorbis/vorbisenc.h ../include/vorbis

clean::
	rm -rf $(VORBIS)


# --- FREETYPE ------------------------

FREETYPE := freetype-2.3.9

freetype:$(FREETYPE)

$(FREETYPE):
	tar xvzf tars/$(FREETYPE).tgz
	cp configs/freetype-makefile $(FREETYPE)/Makefile
	cp patches/freetype/ftconfig.h $(FREETYPE)/include/freetype/config
	cd $(FREETYPE); make
	cp $(FREETYPE)/libfreetype$(SS_LIBEXT) $(LIB_DIR)
	cp $(FREETYPE)/include/ft2build.h ../include
	cp -r $(FREETYPE)/include/freetype ../include

clean::
	rm -rf $(FREETYPE)

# --- SMPEG ------------------------

SMPEG := smpeg

#smpeg:$(SMPEG)

$(SMPEG):
	tar xvzf tars/$(SMPEG).tgz
	cp configs/smpeg-makefile $(SMPEG)/Makefile
	cd $(SMPEG); make
	cp $(SMPEG)/libsmpeg$(SS_LIBEXT) $(LIB_DIR)
	cp $(SMPEG)/smpeg.h $(SMPEG)/MPEGfilter.h ../include/

clean::
	rm -rf $(SMPEG)




# --- SDL ------------------------

ifndef SS_IPHONE
SDL := SDL-1.2.13
	#cp patches/SDL/SDL_win32window.c $(SDL)/src/video/win32
	#cp patches/SDL/SDL_sysvideo.h $(SDL)/src/video
	#cp patches/SDL/SDL_atomic.h $(SDL)/include
SDL_INC := ../include/SDL12
else
SDL := SDL-1.3.0-5441
SDL_INC := ../include/SDL13
endif

sdl:$(SDL)

$(SDL): always
	tar xzf tars/$(SDL).tgz
	mkdir -p $(SDL_INC)
	cp configs/sdl-makefile $(SDL)/Makefile
ifndef SS_IPHONE
	cp patches/SDL/SDL_config* $(SDL)/include
else
	cp patches/SDL-1.3.0-5441/SDL_mouse.c $(SDL)/src/events
	cp patches/SDL-1.3.0-5441/SDL_renderer_gles.c $(SDL)/src/video
endif
	cd $(SDL); make
	cp $(SDL)/libSDL$(SS_LIBEXT) $(LIB_DIR)
ifdef SS_WINDOWS
	cp $(SDL)/libSDLmain$(SS_LIBEXT) $(LIB_DIR)
endif
	cp $(SDL)/include/*.h $(SDL_INC)

clean::
	rm -rf $(SDL)

# --- SDL_TTF ------------------------

SDL_TTF := SDL_ttf-2.0.9

sdl_ttf:$(SDL_TTF)

$(SDL_TTF):
	tar xvzf tars/$(SDL_TTF).tgz
	cp configs/sdl_ttf-makefile $(SDL_TTF)/Makefile
	cd $(SDL_TTF); make
	cp $(SDL_TTF)/libSDL_ttf$(SS_LIBEXT) $(LIB_DIR)
	cp $(SDL_TTF)/SDL_ttf.h ../include

clean::
	rm -rf $(SDL_TTF)

# --- SDL_IMAGE ------------------------

SDL_IMAGE := SDL_image-1.2.7

sdl_image:$(SDL_IMAGE)

$(SDL_IMAGE):
	tar xvzf tars/$(SDL_IMAGE).tgz
	cp configs/sdl_image-makefile $(SDL_IMAGE)/Makefile
	cd $(SDL_IMAGE); make
	cp $(SDL_IMAGE)/libSDL_image$(SS_LIBEXT) $(LIB_DIR)
	cp $(SDL_IMAGE)/SDL_image.h ../include

clean::
	rm -rf $(SDL_IMAGE)


# --- SDL_MIXER ------------------------

SDL_MIXER := SDL_mixer-1.2.8

sdl_mixer:$(SDL_MIXER)

$(SDL_MIXER):
	tar xvzf tars/$(SDL_MIXER).tgz
	cp configs/sdl_mixer-makefile $(SDL_MIXER)/Makefile
	cp patches/SDL_mixer/mixer.c $(SDL_MIXER)
	cp patches/SDL_mixer/SDL_mixer.h $(SDL_MIXER)
	cp patches/SDL_mixer/native_midi_common.c $(SDL_MIXER)/native_midi
	cd $(SDL_MIXER); make
	cp $(SDL_MIXER)/libSDL_mixer$(SS_LIBEXT) $(LIB_DIR)
	cp $(SDL_MIXER)/SDL_mixer.h ../include

clean::
	rm -rf $(SDL_MIXER)



