# FTEQW Makefile to build natively on Apple Silicon host for Apple Silicon target

# Commands

CC  := gcc
CXX := g++
LD  := g++

CD    := cd
RM    := rm
COPY  := cp
RMDIR := rm -r
MKDIR := mkdir -p

BREW_INSTALL := brew install

# Directories

BASE_DIR    := ../engine
RELEASE_DIR := $(BASE_DIR)/release
DEBUG_DIR   := $(BASE_DIR)/debug
CLIENT_DIR  := $(BASE_DIR)/client
SERVER_DIR  := $(BASE_DIR)/server
COMMON_DIR  := $(BASE_DIR)/common
HTTP_DIR    := $(BASE_DIR)/http
GL_DIR      := $(BASE_DIR)/gl
QCLIB_DIR   := $(BASE_DIR)/qclib

# Libraries
# TODO: Remove brew dependency and build libs manually

BREW_PREFIX := $(shell brew --prefix)

LIB_PACKAGES := pkg-config sdl2 libpng libjpeg libvorbis freetype speex speexdsp opus

BREW_L_FLAGS := -L$(BREW_PREFIX)/lib
BREW_C_FLAGS := -I$(BREW_PREFIX)/include

LIBPNG_PATH    := $(subst -L,,$(shell pkg-config libpng --libs-only-L))
LIBPNG_L_FLAGS := -L$(LIBPNG_PATH) $(LIBPNG_PATH)/libpng.a -lz
LIBPNG_C_FLAGS := $(shell pkg-config libpng --cflags)

LIBJPEG_PATH    := $(subst -L,,$(shell pkg-config libjpeg --libs-only-L))
LIBJPEG_L_FLAGS := -L$(LIBJPEG_PATH) $(LIBJPEG_PATH)/libjpeg.a
LIBJPEG_C_FLAGS := $(shell pkg-config libjpeg --cflags)

LIBVORBIS_L_FLAGS := $(shell pkg-config vorbis --libs)
LIBVORBIS_C_FLAGS := $(shell pkg-config vorbis --cflags)

LIBSPEEX_L_FLAGS := $(shell pkg-config speex --libs)
LIBSPEEX_C_FLAGS := $(shell pkg-config speex --cflags)

LIBSPEEXDSP_L_FLAGS := $(shell pkg-config speexdsp --libs)
LIBSPEEXDSP_C_FLAGS := $(shell pkg-config speexdsp --cflags)

LIBOPUS_L_FLAGS := $(shell pkg-config opus --libs)
LIBOPUS_C_FLAGS := $(shell pkg-config opus --cflags)

LIBFREETYPE_L_FLAGS := -L$(BREW_PREFIX)/opt/freetype/lib -lfreetype
LIBFREETYPE_C_FLAGS := -I$(BREW_PREFIX)/opt/freetype/include/freetype2

SDL2_L_FLAGS := $(shell sdl2-config --libs)
SDL2_C_FLAGS := $(shell sdl2-config --cflags)

LIBS_L_FLAGS := $(SDL2_L_FLAGS) $(BREW_L_FLAGS) $(LIBPNG_L_FLAGS) $(LIBJPEG_L_FLAGS) $(LIBVORBIS_L_FLAGS) $(LIBFREETYPE_L_FLAGS) $(LIBSPEEX_L_FLAGS) $(LIBSPEEXDSP_L_FLAGS) $(LIBOPUS_L_FLAGS)
LIBS_C_FLAGS := $(SDL2_C_FLAGS) $(BREW_C_FLAGS) $(LIBPNG_C_FLAGS) $(LIBJPEG_C_FLAGS) $(LIBVORBIS_C_FLAGS) $(LIBFREETYPE_C_FLAGS) $(LIBSPEEX_C_FLAGS) $(LIBSPEEXDSP_C_FLAGS) $(LIBOPUS_C_FLAGS)

# Options

EXE_NAME := fteqw

BASE_INCLUDES    := -I$(CLIENT_DIR) -I$(SERVER_DIR) -I$(COMMON_DIR) -I$(GL_DIR) -I$(QCLIB_DIR) -I. 
WARNINGFLAGS     := -Wall -Wno-pointer-sign -Wno-unknown-pragmas -Wno-format-zero-length -Wno-strict-aliasing
CPUOPTIMIZATIONS := -Os
DEBUG_CFLAGS     := -g -DDEBUG -D_DEBUG
RELEASE_CFLAGS   := $(CPUOPTIMIZATIONS)
GLCFLAGS         := -DGLQUAKE

APPLESILICON_CFLAGS := -DAPPLE_SILICON -DGLQUAKE -DNO_OPENAL -DNO_X11 -DLIBJPEG_STATIC -DLIBPNG_STATIC -DMULTITHREAD
					 # -DOPUS_STATIC -DSPEEX_STATIC -DFREETYPE_STATIC -DLIBVORBISFILE_STATIC

LD_FLAGS  := -lm -ldl -lpthread $(LIBS_L_FLAGS)
C_FLAGS   := $(WARNINGFLAGS) $(DEBUG_CFLAGS) $(GLCFLAGS) $(LIBS_C_FLAGS) $(APPLESILICON_CFLAGS)
CXX_FLAGS := $(C_FLAGS)

# Objects

SDL_OBJS := sys_sdl.o in_sdl.o cd_sdl.o snd_sdl.o

CLIENT_OBJS := textedit.o fragstats.o zqtp.o cl_demo.o cl_ents.o clq2_ents.o cl_input.o in_generic.o cl_main.o \
	cl_parse.o cl_pred.o cl_tent.o cl_cam.o cl_screen.o pr_clcmd.o cl_ignore.o pr_csqc.o console.o image.o keys.o \
	menu.o m_master.o m_multi.o m_items.o m_options.o m_single.o m_script.o m_native.o m_mp3.o roq_read.o clq2_cin.o \
	r_part.o p_script.o p_null.o p_classic.o r_partset.o renderer.o renderque.o sbar.o skin.o snd_dma.o snd_mem.o \
	snd_mix.o snd_mp3.o snd_ov.o valid.o vid_headless.o view.o wad.o pr_menu.o  r_2d.o pr_skelobj.o r_surf.o \
	$(SDL_OBJS)

SERVER_OBJS := pr_cmds.o pr_q1qvm.o pr_lua.o sv_master.o sv_init.o sv_main.o sv_nchan.o sv_ents.o sv_send.o sv_user.o \
	sv_sql.o sv_mvd.o sv_ccmds.o sv_cluster.o sv_rankin.o sv_chat.o sv_demo.o net_preparse.o savegame.o svq2_ents.o \
	svq2_game.o

COMMON_OBJS := gl_alias.o gl_hlmdl.o gl_heightmap.o gl_model.o com_bih.o com_mesh.o common.o json.o cvar.o cmd.o crc.o \
	net_ssl_gnutls.o net_master.o fs.o fs_stdio.o fs_pak.o fs_zip.o fs_dzip.o fs_xz.o m_download.o mathlib.o huff.o md4.o \
	sha1.o sha2.o log.o net_chan.o net_wins.o net_ice.o zone.o qvm.o r_d3.o gl_q2bsp.o glmod_doom.o world.o \
	sv_phys.o sv_move.o pmove.o pmovetst.o translate.o plugin.o q1bsp.o q2pmove.o pr_bgcmd.o

GLQUAKE_OBJS := gl_draw.o gl_backend.o gl_rmain.o gl_rmisc.o gl_rsurf.o gl_screen.o gl_bloom.o gl_vidcommon.o gl_font.o \
	gl_rlight.o gl_vidsdl.o ltface.o gl_shader.o gl_shadow.o gl_warp.o gl_ngraph.o

HTTP_OBJS := ftpclient.o ftpserver.o httpclient.o httpserver.o iwebiface.o webgen.o

QCLIB_OBJS := hash.o initlib.o comprout.o pr_edict.o qcc_cmdlib.o pr_exec.o pr_multi.o qcc_pr_comp.o qcc_pr_lex.o qccmain.o \
	qcd_main.o

# Rules

all: build

brew_install_libs:
	$(BREW_INSTALL) $(LIB_PACKAGES)

compile: $(CLIENT_OBJS) $(SERVER_OBJS) $(COMMON_OBJS) $(GLQUAKE_OBJS) $(HTTP_OBJS) $(QCLIB_OBJS)

build: compile link

install:
	$(COPY) $(EXE_NAME) $(BREW_PREFIX)/bin/

uninstall:
	$(RM) $(BREW_PREFIX)/bin/$(EXE_NAME)

clean:
	-$(RM) *.o
	-$(RM) $(EXE_NAME)

link:
	$(LD) $(CLIENT_OBJS) $(SERVER_OBJS) $(COMMON_OBJS) $(GLQUAKE_OBJS) $(HTTP_OBJS) $(QCLIB_OBJS) $(LD_FLAGS) -o $(EXE_NAME)

# CLIENT_OBJS
%.o: $(CLIENT_DIR)/%.c
	$(CC) $(C_FLAGS) $(BASE_INCLUDES) -c $< -o $@
%.o: $(CLIENT_DIR)/%.cpp
	$(CXX) $(CXX_FLAGS) $(BASE_INCLUDES) -c $< -o $@

# SERVER_OBJS
%.o: $(SERVER_DIR)/%.c
	$(CC) $(C_FLAGS) $(BASE_INCLUDES) -c $< -o $@
%.o: $(SERVER_DIR)/%.cpp
	$(CXX) $(CXX_FLAGS) $(BASE_INCLUDES) -c $< -o $@

# COMMON_OBJS
%.o: $(COMMON_DIR)/%.c
	$(CC) $(C_FLAGS) $(BASE_INCLUDES) -c $< -o $@
%.o: $(COMMON_DIR)/%.cpp
	$(CXX) $(CXX_FLAGS) $(BASE_INCLUDES) -c $< -o $@

# GLQUAKE_OBJS
%.o: $(GL_DIR)/%.c
	$(CC) $(C_FLAGS) $(BASE_INCLUDES) -c $< -o $@
%.o: $(GL_DIR)/%.cpp
	$(CXX) $(CXX_FLAGS) $(BASE_INCLUDES) -c $< -o $@

# HTTP_OBJS
%.o: $(HTTP_DIR)/%.c
	$(CC) $(C_FLAGS) $(BASE_INCLUDES) -c $< -o $@
%.o: $(HTTP_DIR)/%.cpp
	$(CXX) $(CXX_FLAGS) $(BASE_INCLUDES) -c $< -o $@

# QCLIB_OBJS
%.o: $(QCLIB_DIR)/%.c
	$(CC) $(C_FLAGS) $(BASE_INCLUDES) -c $< -o $@
%.o: $(QCLIB_DIR)/%.cpp
	$(CXX) $(CXX_FLAGS) $(BASE_INCLUDES) -c $< -o $@