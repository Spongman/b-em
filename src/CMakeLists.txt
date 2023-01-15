
find_package(ZLIB)


if(MSVC)
	find_package(unofficial-allegro5 CONFIG REQUIRED)
	set(ALLEGRO5_INCLUDE_DIRS ${ZLIB_INCLUDE_DIRS})

else()

	find_package(PkgConfig REQUIRED)

	pkg_check_modules(ALLEGRO5 REQUIRED
		allegro_audio-5
		allegro_acodec-5
		allegro_primitives-5
		allegro_dialog-5
		allegro_image-5
		allegro_font-5
		allegro-5
	)
endif()




################################################
### b-Em

if(MSVC)
	set(BEM_PLATFORM_FILES
		midi-windows.c
	)
else()
	set(BEM_PLATFORM_FILES
		midi-linux.c
	)
endif()


add_executable(b-em WIN32
	6502.c
	6502debug.c
	6502tube.c
	65816.c
  6809tube.c
	NS32016/32016.c
	NS32016/32016_debug.c
	NS32016/Decode.c
	NS32016/NSDis.c
	NS32016/Profile.c
	NS32016/Trap.c
	NS32016/mem32016.c
	z80.c
	z80dis.c
	acia.c
	adc.c
	arm.c
	darm/darm.c
	darm/darm-tbl.c
	darm/armv7.c
	darm/armv7-tbl.c
	darm/thumb.c
	darm/thumb-tbl.c
	darm/thumb2.c
	darm/thumb2-decoder.c
	darm/thumb2-tbl.c
	cmos.c
	compact_joystick.c
	compactcmos.c
	compat_wrappers.c
	config.c
  copro-pdp11.c
	csw.c
	ddnoise.c
	debugger.c
	debugger_symbols.cpp
	disc.c fdi.c
	fdi2raw.c
	fullscreen.c
	gui-allegro.c
	hfe.c
	i8271.c
	ide.c
	imd.c
	joystick.c
	keyboard.c
	keydef-allegro.c
	led.c
	linux.c
	logging.c
  musahi/m68kcpu.c
  musahi/m68kops.c
  musahi/m68kdasm.c
  mc68000tube.c
  mc6809nc/mc6809nc.c
  mc6809nc/mc6809_debug.c
  mc6809nc/mc6809_dis.c
	main.c
	mem.c
	model.c
	mouse.c
	mmccard.c
	music2000.c
	music4000.c
	music5000.c
	paula.c
	pal.c
	resid.cc
	savestate.c
	scsi.c
	sdf-acc.c
	sdf-geo.c
	serial.c
	sn76489.c
	sound.c
	sysacia.c
	sysvia.c
	tape.c
	tapecat-allegro.c
	tapenoise.c
  pdp11/pdp11.c
  pdp11/pdp11_debug.c
	tube.c
	uef.c
	uservia.c
	vdfs.c
	via.c
	vidalleg.c
	video.c
	wd1770.c
	win.c
	x86.c
	x86dasm.c
	resid-fp/convolve-sse.cc
	resid-fp/convolve.cc
	resid-fp/envelope.cc
	resid-fp/extfilt.cc
	resid-fp/filter.cc
	resid-fp/pot.cc
	resid-fp/sid.cc
	resid-fp/voice.cc
	resid-fp/wave.cc
	resid-fp/wave6581_PST.cc
	resid-fp/wave6581_PS_.cc
	resid-fp/wave6581_P_T.cc
	resid-fp/wave6581__ST.cc
	resid-fp/wave8580_PST.cc
	resid-fp/wave8580_PS_.cc
	resid-fp/wave8580_P_T.cc
	resid-fp/wave8580__ST.cc

	${BEM_PLATFORM_FILES}
)

target_compile_definitions(b-em PUBLIC
 BEM
 INCLUDE_DEBUGGER
 USE_MEMORY_POINTER
 VERSION="${PROJECT_VERSION}"
)

target_include_directories(b-em PRIVATE
	${ZLIB_INCLUDE_DIRS}
)

if (MSVC)
	set_property(TARGET b-em PROPERTY MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")

	target_link_libraries(b-em PRIVATE
		ZLIB::ZLIB
		unofficial-allegro5::allegro_audio
		unofficial-allegro5::allegro_acodec
		unofficial-allegro5::allegro_primitives
		unofficial-allegro5::allegro_dialog
		unofficial-allegro5::allegro_image
		unofficial-allegro5::allegro_font
		unofficial-allegro5::allegro
	)
else()

	target_link_libraries(b-em PRIVATE
		${ALLEGRO5_LIBRARIES}
		ZLIB::ZLIB
	)
endif()


################################################
### hdfmt

add_executable(hdfmt
	hdfmt.c
)

set_property(TARGET hdfmt PROPERTY MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")


################################################
### jstest

add_executable(jstest
	jstest.c
)

set_property(TARGET jstest PROPERTY MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")

target_include_directories(jstest PRIVATE
	${ALLEGRO5_INCLUDE_DIRS}
)

target_link_libraries(jstest
	${ALLEGRO5_LIBRARIES}
)

if(MSVC)
target_link_libraries(jstest
	unofficial-allegro5::allegro
)
endif()


################################################
### gtest

add_executable(gtest
	sdf-gtest.c
	sdf-geo.c
)

set_property(TARGET gtest PROPERTY MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")

target_include_directories(gtest PRIVATE
	${ALLEGRO5_INCLUDE_DIRS}
)

target_link_libraries(gtest
	${ALLEGRO5_LIBRARIES}
)

if(MSVC)
target_link_libraries(gtest
	unofficial-allegro5::allegro
)
endif()



################################################
### sdf2imd

add_executable(sdf2imd
	sdf2imd.c
	sdf-geo.c
)

set_property(TARGET sdf2imd PROPERTY MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")

target_include_directories(sdf2imd PRIVATE
	${ALLEGRO5_INCLUDE_DIRS}
)

target_link_libraries(sdf2imd
	${ALLEGRO5_LIBRARIES}
)

if(MSVC)
target_link_libraries(sdf2imd
	unofficial-allegro5::allegro
)
endif()

################################################
### bsnapdump

add_executable(bsnapdump
	bsnapdump.c
)

set_property(TARGET bsnapdump PROPERTY MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")

target_link_libraries(bsnapdump
	ZLIB::ZLIB
)



install(
	TARGETS
		b-em
		hdfmt
		jstest
		gtest
		sdf2imd
		bsnapdump
	DESTINATION bin
)
