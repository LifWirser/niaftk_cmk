cmake_minimum_required(VERSION 3.19.2)
project(naif-tk) 

set(NAIF-TK_VERSION N0067)
set(NAIF-TK_DESCRIPTION see docs for more) 

set(PROJECT_C_STANDARD 90)
set(CMAKE_C_FLAGS "-m64 " ) 

# Link stdc++, libpqxx, and libpq
set(CMAKE_EXE_LINKER_FLAGS "-lm -m64")

#require f2c libf2c-dev libmath maybe more
#
# Pick what to build

option(NIAF-TK_BUILD_SHARED_LIBRARY "Build NIAF-TK library as static library" ON)
message(STATUS "Build Shared Library = " ${NIAF-TK_BUILD_SHARED_LIBRARY})
option(NIAF-TK_BUILD_STATIC_LIBRARY "Build NIAF-TK library as static library" ON)
message(STATUS "Build Static Library = " ${NIAF-TK_BUILD_STATIC_LIBRARY})
#BUILD_COOKBOOK using zsame rules as executables
option(BUILD_COOKBOOK OFF)
message(STATUS "Cook book samples needing work" ${BUILD_COOKBOOK})

#Set base-source

set(INCLUDE_PATH "${PROJECT_SOURCE_DIR}/include")
set(PGM_TMP "${PROJECT_SOURCE_DIR}/pgm_tmp/")
message(STATUS "list PGM_TMP " ${PGM_TMP})
file(MAKE_DIRECTORY ${PGM_TMP})

# In as much as I desired to = NO_GLOB, this source has 2229 c source files
file(GLOB NIAF-TK_COMMON_SOURCE ${PROJECT_SOURCE_DIR}/src/cspice/*.c)

# Now to connect code from f2c conversion with main.x 
# producing what the static libs get linked into resulting in the executables
# It AIN'T PRETTY but it WORKS

file(COPY "${PROJECT_SOURCE_DIR}/src/brief_c/main.x" DESTINATION ${PGM_TMP})
#execute_process (COMMAND bash -c "cp /home/enon/build/cspice/src/brief_c/main.x ${PROJECT_SOURCE_DIR}/pgm_tmp" )
execute_process (COMMAND bash -c "cp /home/enon/build/cspice/src/*/*.pgm ${PROJECT_SOURCE_DIR}/pgm_tmp" )

# This copies all *.pgm file to be made into program c code, ie with main function and functions converted from fortran

#function(cat IN_FILE OUT_FILE)
 # file(READ ${IN_FILE} CONTENTS)
 # file(APPEND ${OUT_FILE} "${CONTENTS}")
#endfunction()
#file(WRITE tmp.c "")

# Call the "cat" function for each input file
#foreach(PGM_LIST ${PGM_LISTS})
 # cat(${PGM_LIST} tmp.c )
#endforeach()

execute_process (COMMAND bash -c "pwd")
file(APPEND ../pgm_tmp/main.x ../pgm_tmp/brief.pgm OUTPUT_FILE ../pgm_tmp/brief.c)
#execute_process (COMMAND bash -c "cat ../pgm_tmp/main.x ../pgm_tmp/brief.pgm" OUTPUT_FILE ../pgm_tmp/brief.c)
execute_process (COMMAND bash -c "cat ../pgm_tmp/main.x ../pgm_tmp/dskbrief.pgm" OUTPUT_FILE ../pgm_tmp/dskbrief.c)
execute_process (COMMAND bash -c "cat ../pgm_tmp/main.x ../pgm_tmp/dskexp.pgm" OUTPUT_FILE ../pgm_tmp/dskexp.c)
execute_process (COMMAND bash -c "cat ../pgm_tmp/main.x ../pgm_tmp/chronos.pgm" OUTPUT_FILE ../pgm_tmp/chronos.c)
execute_process (COMMAND bash -c "cat ../pgm_tmp/main.x ../pgm_tmp/ckbrief.pgm " OUTPUT_FILE ../pgm_tmp/chbrief.c)
execute_process (COMMAND bash -c "cat ../pgm_tmp/main.x ../pgm_tmp/commnt.pgm" OUTPUT_FILE ../pgm_tmp/commnt.c)
execute_process (COMMAND bash -c "cat ../pgm_tmp/main.x ../pgm_tmp/states.pgm" OUTPUT_FILE ../pgm_tmp/states.c)
execute_process (COMMAND bash -c "cat ../pgm_tmp/main.x ../pgm_tmp/simple.pgm" OUTPUT_FILE ../pgm_tmp/simple.c)
execute_process (COMMAND bash -c "cat ../pgm_tmp/main.x ../pgm_tmp/subpt.pgm" OUTPUT_FILE ../pgm_tmp/subpt.c)
execute_process (COMMAND bash -c "cat ../pgm_tmp/main.x ../pgm_tmp/tictoc.pgm" OUTPUT_FILE ../pgm_tmp/tictoc.c)
execute_process (COMMAND bash -c "cat ../pgm_tmp/main.x ../pgm_tmp/frmdiff.pgm" OUTPUT_FILE ../pgm_tmp/frmdiff.c)
execute_process (COMMAND bash -c "cat ../pgm_tmp/main.x ../pgm_tmp/inspekt.pgm" OUTPUT_FILE ../pgm_tmp/inspekt.c)
execute_process (COMMAND bash -c "cat ../pgm_tmp/main.x ../pgm_tmp/mkdsk.pgm" OUTPUT_FILE ../pgm_tmp/mkdsk.c)
execute_process (COMMAND bash -c "cat ../pgm_tmp/main.x ../pgm_tmp/mkspk.pgm" OUTPUT_FILE ../pgm_tmp/mkspk.c)
execute_process (COMMAND bash -c "cat ../pgm_tmp/main.x ../pgm_tmp/msopck.pgm" OUTPUT_FILE ../pgm_tmp/msopck.c)
execute_process (COMMAND bash -c "cat ../pgm_tmp/main.x ../pgm_tmp/spacit.pgm" OUTPUT_FILE ../pgm_tmp/spacit.c)
execute_process (COMMAND bash -c "cat ../pgm_tmp/main.x ../pgm_tmp/spkdiff.pgm" OUTPUT_FILE ../pgm_tmp/spkdiff.c)
execute_process (COMMAND bash -c "cat ../pgm_tmp/main.x ../pgm_tmp/spkmerge.pgm" OUTPUT_FILE ../pgm_tmp/spkmerge.c)
execute_process (COMMAND bash -c "cat ../pgm_tmp/main.x ../pgm_tmp/tobin.pgm" OUTPUT_FILE ../pgm_tmp/tobin.c)
execute_process (COMMAND bash -c "cat ../pgm_tmp/main.x ../pgm_tmp/toxfr.pgm" OUTPUT_FILE ../pgm_tmp/toxfr.c)
execute_process (COMMAND bash -c "cat ../pgm_tmp/main.x ../pgm_tmp/version.pgm" OUTPUT_FILE ../pgm_tmp/version.c)

# more for show atm, both are needed for executables

if(${NIAF-TK_BUILD_SHARED_LIBRARY})

add_library(cspice SHARED 
 	"${NIAF-TK_COMMON_SOURCE}")
# You REALLY DON'T want all these listed - 2229 ".c" files
target_link_libraries(cspice -lm )
target_include_directories(cspice PUBLIC ${INCLUDE_PATH})

add_library(csupport SHARED 
# just a small taste ~8% left from here
	../src/csupport/batch.c ../src/csupport/m2core.c ../src/csupport/podbgc.c
	../src/csupport/bboard_1.c ../src/csupport/m2day.c ../src/csupport/podbgd.c
	../src/csupport/bestwd.c ../src/csupport/m2diag.c ../src/csupport/podbgi.c
	../src/csupport/builtn.c ../src/csupport/m2engl.c ../src/csupport/podcgc.c
	../src/csupport/cbget_1.c ../src/csupport/m2epoc.c ../src/csupport/podcgd.c
	../src/csupport/cbinit_1.c ../src/csupport/m2geta.c ../src/csupport/podcgi.c
	../src/csupport/cbput_1.c ../src/csupport/m2getb.c ../src/csupport/poddgc.c
	../src/csupport/cbrem_1.c ../src/csupport/m2getc.c ../src/csupport/poddgd.c
	../src/csupport/changu.c ../src/csupport/m2getd.c ../src/csupport/poddgi.c
	../src/csupport/chbfit.c ../src/csupport/m2geti.c ../src/csupport/podegc.c
	../src/csupport/ck3sdn.c ../src/csupport/m2gmch.c ../src/csupport/podegd.c
	../src/csupport/cmloop.c ../src/csupport/m2have.c ../src/csupport/podegi.c
	../src/csupport/cmmore.c ../src/csupport/m2int.c ../src/csupport/podiec.c
	../src/csupport/cmredo.c ../src/csupport/m2ints.c ../src/csupport/podied.c
	../src/csupport/cmstup.c ../src/csupport/m2keyw.c ../src/csupport/podiei.c
	../src/csupport/cnfirm_1.c ../src/csupport/m2mon.c ../src/csupport/podonc.c
	../src/csupport/cnfirm.c ../src/csupport/m2name.c ../src/csupport/podond.c
	../src/csupport/convbt.c ../src/csupport/m2ntem.c ../src/csupport/podoni.c
	../src/csupport/convrt_2.c ../src/csupport/m2numb.c ../src/csupport/podrec.c
	../src/csupport/convrt_3.c ../src/csupport/m2pars.c ../src/csupport/podred.c
	../src/csupport/convtb.c ../src/csupport/m2selb.c ../src/csupport/podrei.c
	../src/csupport/cputim.c ../src/csupport/m2selc.c ../src/csupport/podrgc.c
	../src/csupport/crtptr.c ../src/csupport/m2seld.c ../src/csupport/podrgd.c
	../src/csupport/curtim.c ../src/csupport/m2seli.c ../src/csupport/podrgi.c
	../src/csupport/cutstr.c ../src/csupport/m2shll.c ../src/csupport/prcomf.c
	../src/csupport/dafacu.c ../src/csupport/m2term.c ../src/csupport/prepsn.c
	../src/csupport/dafecu.c ../src/csupport/m2thnq.c ../src/csupport/prtrap.c
	../src/csupport/dcyphr.c ../src/csupport/m2time.c ../src/csupport/pstack.c
	../src/csupport/dimcb_1.c ../src/csupport/m2tran.c ../src/csupport/qlstnb.c
	../src/csupport/dpstre.c ../src/csupport/m2trim.c ../src/csupport/qmini.c
	../src/csupport/dspvrs.c ../src/csupport/m2unit.c ../src/csupport/qrtrim.c
	../src/csupport/echo.c ../src/csupport/m2wmch.c ../src/csupport/qtran.c
	../src/csupport/edtcmd.c ../src/csupport/m2xist.c ../src/csupport/rc2grd.c
	../src/csupport/edtcom.c ../src/csupport/m2year.c ../src/csupport/rdstmn.c
	../src/csupport/exesys.c ../src/csupport/makstr.c ../src/csupport/rdstmt.c
	../src/csupport/expfnm_1.c ../src/csupport/match.c ../src/csupport/reglon.c
	../src/csupport/expfnm_2.c ../src/csupport/matchc.c ../src/csupport/ressym.c
	../src/csupport/flgrpt.c ../src/csupport/matche.c ../src/csupport/rptsym.c
	../src/csupport/fndcmp.c ../src/csupport/matchm.c ../src/csupport/sbget_1.c
	../src/csupport/fndntk.c ../src/csupport/matcho.c ../src/csupport/sbinit_1.c
	../src/csupport/fndptk.c ../src/csupport/meta_2.c ../src/csupport/sbrem_1.c
	../src/csupport/fnducv.c ../src/csupport/mspeld.c ../src/csupport/sbset_1.c
	../src/csupport/getcml.c ../src/csupport/ncodec.c ../src/csupport/scansl.c
	../src/csupport/getdel.c ../src/csupport/ncoded.c ../src/csupport/shosym.c
	../src/csupport/geteq.c ../src/csupport/ncodei.c ../src/csupport/sizecb_1.c
	../src/csupport/getfnm_1.c ../src/csupport/newfil_1.c ../src/csupport/spcacb.c
	../src/csupport/getfnm.c ../src/csupport/newfil.c ../src/csupport/spcacc.c
	../src/csupport/getopt_1.c ../src/csupport/nicebt_1.c ../src/csupport/stran.c
	../src/csupport/getopt_2.c ../src/csupport/niceio_3.c ../src/csupport/syptrc.c
	../src/csupport/getopt.c ../src/csupport/nicepr_1.c ../src/csupport/syptri.c
	../src/csupport/have.c ../src/csupport/no.c ../src/csupport/tabrpt.c
	../src/csupport/header.c ../src/csupport/nspio.c ../src/csupport/trnlat.c
	../src/csupport/iovcmp.c ../src/csupport/nsplgr.c ../src/csupport/txtops.c
	../src/csupport/langua.c ../src/csupport/nspopl.c ../src/csupport/unitp.c
	../src/csupport/lbdes_1.c ../src/csupport/nsppwd.c ../src/csupport/upto.c
	../src/csupport/lbget_1.c ../src/csupport/nspsav.c ../src/csupport/utrans_2.c
	../src/csupport/lbinit_1.c ../src/csupport/nspxcp.c ../src/csupport/zzcapplt.c
	../src/csupport/lbins_1.c ../src/csupport/nthuqt.c ../src/csupport/zzckcvr2.c
	../src/csupport/lbpack_1.c ../src/csupport/nthuqw.c ../src/csupport/zzckcvr3.c
	../src/csupport/lbrem_1.c ../src/csupport/nxtcom.c ../src/csupport/zzckcvr4.c
	../src/csupport/lbupd_1.c ../src/csupport/occurs.c ../src/csupport/zzckcvr5.c
	../src/csupport/logchk.c ../src/csupport/pagman.c ../src/csupport/zzcke06.c
	../src/csupport/m2alph.c ../src/csupport/parcml.c ../src/csupport/zzdbrgap.c
	../src/csupport/m2begr.c ../src/csupport/pltfrm.c ../src/csupport/zzellplt.c
	../src/csupport/m2bodini.c ../src/csupport/podaec.c ../src/csupport/zzellsec.c
	../src/csupport/m2bodtrn.c ../src/csupport/podaed.c ../src/csupport/zzgetenv.c
	../src/csupport/m2body.c ../src/csupport/podaei.c ../src/csupport/zzgetfat.c
	../src/csupport/m2cal.c ../src/csupport/podbec.c ../src/csupport/zzgrdplt.c
	../src/csupport/m2chck.c ../src/csupport/podbed.c ../src/csupport/zznsppok.c
	../src/csupport/m2clss.c ../src/csupport/podbei.c ../src/csupport/zztxtopn.c)
target_link_libraries(csupport -lm cspice )
target_include_directories(csupport PUBLIC ${INCLUDE_PATH})

endif()

if(${NIAF-TK_BUILD_STATIC_LIBRARY})

add_library(brief_c STATIC ./src/brief_c/objcf1.c ./src/brief_c/objget.c 
	./src/brief_c/objrem.c ./src/brief_c/objval.c ./src/brief_c/disply.c 
	./src/brief_c/maknam.c ./src/brief_c/objcf2.c ./src/brief_c/objinl.c
	./src/brief_c/objsbc.c ./src/brief_c/prname.c ./src/brief_c/distim.c
	./src/brief_c/objchk.c ./src/brief_c/objmod.c ./src/brief_c/objsbf.c 
	./src/brief_c/rndem.c ./src/brief_c/filtem.c ./src/brief_c/objact.c 
	./src/brief_c/objcmp.c ./src/brief_c/objnth.c ./src/brief_c/objset.c 
	./src/brief_c/writit.c ./src/brief_c/getnam.c ./src/brief_c/objadd.c 
	./src/brief_c/objfnd.c ./src/brief_c/objnxt.c ./src/brief_c/objsiz.c)
target_link_libraries(brief_c -lm )
target_include_directories(brief_c PRIVATE ${INCLUDE_PATH})

add_library(chrnos_c STATIC
	./src/chrnos_c/cronos.c ./src/chrnos_c/ls.c ./src/chrnos_c/speakr.c
	./src/chrnos_c/crcnst.c ./src/chrnos_c/dsplay.c ./src/chrnos_c/lstmid.c)
target_link_libraries(chrnos_c -lm )
target_include_directories(chrnos_c PRIVATE ${INCLUDE_PATH})

add_library(ckbref_c STATIC
	./src/ckbref_c/fixuni.c ./src/ckbref_c/repmcw.c ./src/ckbref_c/dispsm.c 
	./src/ckbref_c/prinst.c ./src/ckbref_c/timecn.c)
target_include_directories(ckbref_c PRIVATE ${INCLUDE_PATH})
target_link_libraries(ckbref_c -lm )

add_library(commnt_c STATIC ./src/commnt_c/clcomm.c)
target_include_directories(commnt_c PRIVATE ${INCLUDE_PATH})
target_link_libraries(commnt_c -lm )

add_library(dskbrief_c STATIC
	./src/dskbrief_c/attcmp.c ./src/dskbrief_c/dskd04.c ./src/dskbrief_c/dspgap.c ./src/dskbrief_c/sum02.c
	./src/dskbrief_c/cortab.c ./src/dskbrief_c/dski04.c ./src/dskbrief_c/grpseg.c ./src/dskbrief_c/sum04.c
	./src/dskbrief_c/dskb04.c ./src/dskbrief_c/dspdsc.c ./src/dskbrief_c/prcinf.c)
target_include_directories(dskbrief_c PRIVATE ${INCLUDE_PATH})
target_link_libraries(dskbrief_c -lm )

add_library(frmdif_c STATIC
	./src/frmdif_c/chwcml.c ./src/frmdif_c/dr2str.c ./src/frmdif_c/ldklst.c
	./src/frmdif_c/ckcovr.c ./src/frmdif_c/et2str.c ./src/frmdif_c/rtdiff.c
	./src/frmdif_c/dpstrp.c ./src/frmdif_c/getqav.c ./src/frmdif_c/sc01s2d.c)
target_link_libraries(frmdif_c -lm )
target_include_directories(frmdif_c PRIVATE ${INCLUDE_PATH})

add_library(inspkt_c STATIC
	./src/inspkt_c/bgroup_1.c ./src/inspkt_c/nspfrp.c ./src/inspkt_c/title.c ./src/inspkt_c/zzhlp015.c
	./src/inspkt_c/chunk.c ./src/inspkt_c/nsphi.c ./src/inspkt_c/tokens.c ./src/inspkt_c/zzhlp016.c
	./src/inspkt_c/clmgr.c ./src/inspkt_c/nsphlp.c ./src/inspkt_c/var.c ./src/inspkt_c/zzhlp017.c
	./src/inspkt_c/expand.c ./src/inspkt_c/nspint.c ./src/inspkt_c/wrtnpr.c ./src/inspkt_c/zzhlp018.c
	./src/inspkt_c/fetcha.c ./src/inspkt_c/nspset.c ./src/inspkt_c/wrtprs.c ./src/inspkt_c/zzhlp019.c
	./src/inspkt_c/fgroup_1.c ./src/inspkt_c/nspshc.c ./src/inspkt_c/zzhlp000.c ./src/inspkt_c/zzhlp020.c
	./src/inspkt_c/fmtdp.c ./src/inspkt_c/nspsho.c ./src/inspkt_c/zzhlp001.c ./src/inspkt_c/zzhlp021.c
	./src/inspkt_c/fmtint.c ./src/inspkt_c/nsptab.c ./src/inspkt_c/zzhlp002.c ./src/inspkt_c/zzhlp022.c
	./src/inspkt_c/fmttim.c ./src/inspkt_c/nsptv.c ./src/inspkt_c/zzhlp003.c ./src/inspkt_c/zzhlp023.c
	./src/inspkt_c/hlpmen.c ./src/inspkt_c/nspvrb.c ./src/inspkt_c/zzhlp004.c ./src/inspkt_c/zzhlp024.c
	./src/inspkt_c/hlptxt.c ./src/inspkt_c/other.c ./src/inspkt_c/zzhlp005.c ./src/inspkt_c/zzhlp025.c
	./src/inspkt_c/inspkn.c ./src/inspkt_c/param.c ./src/inspkt_c/zzhlp006.c ./src/inspkt_c/zzhlp026.c
	./src/inspkt_c/kerman.c ./src/inspkt_c/params.c ./src/inspkt_c/zzhlp007.c ./src/inspkt_c/zzhlp027.c
	./src/inspkt_c/list.c ./src/inspkt_c/prep.c ./src/inspkt_c/zzhlp008.c ./src/inspkt_c/zzhlp028.c
	./src/inspkt_c/liter.c ./src/inspkt_c/preprc.c ./src/inspkt_c/zzhlp009.c ./src/inspkt_c/zzhlp029.c
	./src/inspkt_c/namxpn.c ./src/inspkt_c/proc.c ./src/inspkt_c/zzhlp010.c ./src/inspkt_c/zzhlp030.c
	./src/inspkt_c/nspcht.c ./src/inspkt_c/setchr.c ./src/inspkt_c/zzhlp011.c ./src/inspkt_c/zzhlp031.c
	./src/inspkt_c/nspdel.c ./src/inspkt_c/size.c ./src/inspkt_c/zzhlp012.c ./src/inspkt_c/zzhlp032.c
	./src/inspkt_c/nspflg.c ./src/inspkt_c/subtex.c ./src/inspkt_c/zzhlp013.c
	./src/inspkt_c/nspfnd.c ./src/inspkt_c/tempb.c ./src/inspkt_c/zzhlp014.c)
target_include_directories(inspkt_c PRIVATE ${INCLUDE_PATH})
target_link_libraries(inspkt_c -lm )

add_library(mkdsk_c STATIC
	./src/mkdsk_c//addcom.c ./src/mkdsk_c//prcinf.c ./src/mkdsk_c//rdffpl.c 
	./src/mkdsk_c//zzvoxscl.c ./src/mkdsk_c//extmsi.c ./src/mkdsk_c//prcset.c
	./src/mkdsk_c//rdgrd5.c ./src/mkdsk_c//zzwseg02.c ./src/mkdsk_c//makvtx.c 
	./src/mkdsk_c//prscml.c ./src/mkdsk_c//wrtdsk.c ./src/mkdsk_c//mkgrid.c
	./src/mkdsk_c//rc2cor.c ./src/mkdsk_c//zzpsxtnt.c ./src/mkdsk_c//mkvarr.c 
	./src/mkdsk_c//rdffdi.c ./src/mkdsk_c//zztrgnvx.c)
target_include_directories(mkdsk_c PRIVATE ${INCLUDE_PATH})
target_link_libraries(mkdsk_c -lm )

add_library(mkspk_c STATIC
	./src/mkspk_c/chckdo.c ./src/mkspk_c/parsdo.c ./src/mkspk_c/reorbd.c ./src/mkspk_c/setup.c
	./src/mkspk_c/cmlarg.c ./src/mkspk_c/redbuf.c ./src/mkspk_c/setelm.c ./src/mkspk_c/tle2spk.c)
target_link_libraries(mkspk_c -lm )
target_include_directories(mkspk_c PRIVATE ${INCLUDE_PATH})

add_library(msopck_c STATIC
	./src/msopck_c/linrot_m.c ./src/msopck_c/mkfclk.c ./src/msopck_c/zzmckdmp.c)
target_include_directories(msopck_c PRIVATE ${INCLUDE_PATH})
target_link_libraries(msopck_c -lm )

add_library(spacit_c STATIC
	./src/spacit_c/ckgss.c ./src/spacit_c/pckwss.c ./src/spacit_c/spat2b.c ./src/spacit_c/sumpck.c
	./src/spacit_c/ckwss.c ./src/spacit_c/spab2t.c ./src/spacit_c/spkgss.c ./src/spacit_c/sumspk.c
	./src/spacit_c/getchr.c ./src/spacit_c/spalog.c ./src/spacit_c/spkwss.c ./src/spacit_c/zzconvtb.c
	./src/spacit_c/getint.c ./src/spacit_c/spardc.c ./src/spacit_c/sumck.c
	./src/spacit_c/pckgss.c ./src/spacit_c/spasum.c ./src/spacit_c/sumek.c)
target_include_directories(spacit_c PRIVATE ${INCLUDE_PATH})
target_link_libraries(spacit_c -lm )

add_library(spkdif_c STATIC
	./src/spkdif_c/chwcml.c ./src/spkdif_c/getsta.c ./src/spkdif_c/stdiff.c
	./src/spkdif_c/dr2str.c ./src/spkdif_c/ldklst.c)
target_include_directories(spkdif_c PRIVATE ${INCLUDE_PATH})
target_link_libraries(spkdif_c -lm )

add_library(spkmrg_c STATIC
	./src/spkmrg_c/cparse_2.c ./src/spkmrg_c/rdcmd.c ./src/spkmrg_c/wrdnln.c)
target_include_directories(spkmrg_c PRIVATE ${INCLUDE_PATH})
target_link_libraries(spkmrg_c -lm )

add_library(tobin_c STATIC ./src/tobin_c/zzconvtb.c)
target_include_directories(tobin_c PRIVATE ${INCLUDE_PATH})
target_link_libraries(tobin_c -lm )

endif()

add_executable(brief ../pgm_tmp/brief.c)
target_link_libraries(brief -lf2c -lm brief_c csupport cspice )
  
add_executable(chronos ../pgm_tmp/chronos.c)
target_link_libraries(chronos -lf2c -lm chrnos_c csupport cspice )

add_executable(commnt ../pgm_tmp/commnt.c) 
target_link_libraries(commnt -lf2c -lm commnt_c csupport cspice )

add_executable(dskbrief ../pgm_tmp/dskbrief.c)
target_link_libraries(dskbrief -lf2c -lm dskbrief_c csupport cspice )

add_executable(dskexp ../pgm_tmp/dskexp.c) 
target_link_libraries(dskexp -lf2c -lm csupport cspice )

add_executable(frmdif ../pgm_tmp/frmdiff.c) 
target_link_libraries(frmdif -lf2c -lm frmdif_c csupport cspice )

add_executable(inspkt ../pgm_tmp/inspekt.c)
target_link_libraries(inspkt -lf2c -lm inspkt_c csupport cspice )

add_executable(mkdsk ../pgm_tmp/mkdsk.c)
target_link_libraries(mkdsk -lf2c -lm mkdsk_c csupport cspice )

add_executable(mkspk ../pgm_tmp/mkspk.c)
target_link_libraries(mkspk -lf2c -lm mkspk_c csupport cspice )

add_executable(msopck ../pgm_tmp/msopck.c) 
target_link_libraries(msopck -lf2c -lm msopck_c csupport cspice )

add_executable(spacit ../pgm_tmp/spacit.c)
target_link_libraries(spacit -lf2c -lm spacit_c csupport cspice )

add_executable(spkdiff ../pgm_tmp/spkdiff.c)
target_link_libraries(spkdiff -lf2c -lm spkdif_c csupport cspice )

add_executable(spkmerge ../pgm_tmp/spkmerge.c)
target_link_libraries(spkmerge -lf2c -lm  spkmrg_c csupport cspice )

add_executable(tobin ../pgm_tmp/tobin.c)
target_link_libraries(tobin -lf2c -lm tobin_c csupport cspice )
#add_executable(tobin ../pgm_tmp/tobin.c
#		DIRECTORY ./src/tobin_c/
#		FILES_MATCHING PATTERN "*.c" ) 
#target_link_libraries(tobin -lf2c -lm csupport cspice )

add_executable(toxfr ../pgm_tmp/toxfr.c)
target_link_libraries(toxfr -lf2c -lm csupport cspice )

add_executable(version ../pgm_tmp/version.c)
target_link_libraries(version -lf2c -lm csupport cspice )


if(BUILD_COOKBOOK)

add_executable(simple ../pgm_tmp/simple.c)
target_link_libraries(simple -lf2c -lm csupport cspice )

add_executable(states ../pgm_tmp/states.c)
target_link_libraries(states -lf2c -lm csupport cspice )

add_executable(subpt ../pgm_tmp/subpt.c)
target_link_libraries(subpt -lf2c -lmcsupport cspice )

add_executable(tictoc ../pgm_tmp/tictoc.c)
target_link_libraries(tictoc -lf2c -lm csupport cspice )

endif()
# last remove pgm_tmp
# file(REMOVE_RECURSE ${PROJECT_SOURCE_DIR}/pgm_tmp/)
# OK this doesn't work for now it removes it before build WTF???

install(TARGETS cspice csupport
 	DESTINATION lib/)
install(TARGETS brief chronos commnt dskbrief dskexp frmdif
		inspkt msopck spacit spkdiff spkmerge tobin toxfr version  
 	DESTINATION bin/)
INSTALL (DIRECTORY ${INCLUDE_PATH} 
	DESTINATION include/naif_toolkit
	FILES_MATCHING 
	PATTERN "*.h")
INSTALL (DIRECTORY ${INCLUDE_PATH} DESTINATION share/naif_toolkit
	FILES_MATCHING
	PATTERN "*.c" 
	PATTERN "*.pgm")
INSTALL (DIRECTORY ./data 
	DESTINATION share/naif_toolkit/data)
INSTALL (DIRECTORY ./doc 
	DESTINATION share/naif_toolkit/doc)

