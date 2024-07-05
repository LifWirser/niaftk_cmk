cmake_minimum_required(VERSION 3.19.2)
project(naif-tk) 

set(NAIF-TK_VERSION N0067)
set(NAIF-TK_DESCRIPTION see docs for more) 

enable_language(Fortran)
set(FFLAGS "-m64 -std=legacy -fallow-argument-mismatch")
#F_COMPILEOPTIONS = "-m64 -c -std=legacy -fallow-argument-mismatch -Wno-character-truncation"
# Pick what to build

option(NIAF-TK_BUILD_SHARED_LIBRARY "Build NIAF-TK library as static library" ON)
message(STATUS "Build Shared Library = " ${NIAF-TK_BUILD_SHARED_LIBRARY})
option(NIAF-TK_BUILD_STATIC_LIBRARY "Build NIAF-TK library as static library" ON)
message(STATUS "Build Static Library = " ${NIAF-TK_BUILD_STATIC_LIBRARY})
#BUILD_COOKBOOK using same rules as executables
option(BUILD_COOKBOOK OFF)
message(STATUS "Cook book samples needing work" ${BUILD_COOKBOOK})

#Set base-source
set(INCLUDE_PATH "${PROJECT_SOURCE_DIR}/include")

set(PGM_TMP "${PROJECT_SOURCE_DIR}/pgm_tmp/")
message(STATUS "list PGM_TMP " ${PGM_TMP})
file(MAKE_DIRECTORY ${PGM_TMP})
file(MAKE_DIRECTORY include)

# In as much as I desired to = NO_GLOB, this source has 2229 c source files
file(GLOB NIAF-TK_COMMON_SOURCE ${PROJECT_SOURCE_DIR}/src/spicelib/*.f)


# It AIN'T PRETTY but it WORKS

execute_process (COMMAND bash -c "cp /home/enon/build/toolkit/src/*/*.inc ${PROJECT_SOURCE_DIR}/include" )
execute_process (COMMAND bash -c "cp /home/enon/build/toolkit/src/*/*.pgm ${PROJECT_SOURCE_DIR}/pgm_tmp" )

execute_process (COMMAND bash -c "pwd")
execute_process (COMMAND bash -c "cp ../pgm_tmp/brief.pgm ../pgm_tmp/brief.f")
execute_process (COMMAND bash -c "cp ../pgm_tmp/dskbrief.pgm ../pgm_tmp/dskbrief.f")
execute_process (COMMAND bash -c "cp ../pgm_tmp/dskexp.pgm ../pgm_tmp/dskexp.f")
execute_process (COMMAND bash -c "cp ../pgm_tmp/chronos.pgm ../pgm_tmp/chronos.f")
execute_process (COMMAND bash -c "cp ../pgm_tmp/ckbrief.pgm  ../pgm_tmp/chbrief.f")
execute_process (COMMAND bash -c "cp ../pgm_tmp/commnt.pgm ../pgm_tmp/commnt.f")
execute_process (COMMAND bash -c "cp ../pgm_tmp/states.pgm ../pgm_tmp/states.f")
execute_process (COMMAND bash -c "cp ../pgm_tmp/simple.pgm ../pgm_tmp/simple.f")
execute_process (COMMAND bash -c "cp ../pgm_tmp/subpt.pgm ../pgm_tmp/subpt.f")
execute_process (COMMAND bash -c "cp ../pgm_tmp/tictoc.pgm ../pgm_tmp/tictoc.f")
execute_process (COMMAND bash -c "cp ../pgm_tmp/frmdiff.pgm ../pgm_tmp/frmdiff.f")
execute_process (COMMAND bash -c "cp ../pgm_tmp/inspekt.pgm ../pgm_tmp/inspekt.f")
execute_process (COMMAND bash -c "cp ../pgm_tmp/mkdsk.pgm ../pgm_tmp/mkdsk.f")
execute_process (COMMAND bash -c "cp ../pgm_tmp/mkspk.pgm ../pgm_tmp/mkspk.f")
execute_process (COMMAND bash -c "cp ../pgm_tmp/msopck.pgm ../pgm_tmp/msopck.f")
execute_process (COMMAND bash -c "cp ../pgm_tmp/spacit.pgm ../pgm_tmp/spacit.f")
execute_process (COMMAND bash -c "cp ../pgm_tmp/spkdiff.pgm ../pgm_tmp/spkdiff.f")
execute_process (COMMAND bash -c "cp ../pgm_tmp/spkmerge.pgm ../pgm_tmp/spkmerge.f")
execute_process (COMMAND bash -c "cp ../pgm_tmp/tobin.pgm ../pgm_tmp/tobin.f")
execute_process (COMMAND bash -c "cp ../pgm_tmp/toxfr.pgm ../pgm_tmp/toxfr.f")
execute_process (COMMAND bash -c "cp ../pgm_tmp/version.pgm ../pgm_tmp/version.f")

# more for show atm, both are needed for executables

if(${NIAF-TK_BUILD_SHARED_LIBRARY})

add_library(spice SHARED 
 	"${NIAF-TK_COMMON_SOURCE}")
# You REALLY DON'T want all these listed - 1478 ".f" files
target_link_libraries(spice -lm )

add_library(support SHARED 
# just a small taste ~8% left from here
../src/support/batch.f  ../src/support/m2core.f ../src/support/podbgc.f
../src/support/bboard_1.f  ../src/support/m2day.f  ../src/support/podbgd.f
../src/support/bestwd.f ../src/support/m2diag.f ../src/support/podbgi.f
../src/support/builtn.f ../src/support/m2engl.f ../src/support/podcgc.f
../src/support/cbget_1.f   ../src/support/m2epoc.f ../src/support/podcgd.f
../src/support/cbinit_1.f  ../src/support/m2geta.f ../src/support/podcgi.f
../src/support/cbput_1.f   ../src/support/m2getb.f ../src/support/poddgc.f
../src/support/cbrem_1.f   ../src/support/m2getc.f ../src/support/poddgd.f
../src/support/changu.f ../src/support/m2getd.f ../src/support/poddgi.f
../src/support/chbfit.f ../src/support/m2geti.f ../src/support/podegc.f
../src/support/ck3sdn.f ../src/support/m2gmch.f ../src/support/podegd.f
../src/support/cmloop.f ../src/support/m2have.f ../src/support/podegi.f
../src/support/cmmore.f ../src/support/m2int.f  ../src/support/podiec.f
../src/support/cmredo.f ../src/support/m2ints.f ../src/support/podied.f
../src/support/cmstup.f ../src/support/m2keyw.f ../src/support/podiei.f
../src/support/cnfirm_1.f  ../src/support/m2mon.f  ../src/support/podonc.f
../src/support/cnfirm.f ../src/support/m2name.f ../src/support/podond.f
../src/support/convbt.f ../src/support/m2ntem.f ../src/support/podoni.f
../src/support/convrt_2.f  ../src/support/m2numb.f ../src/support/podrec.f
../src/support/convrt_3.f  ../src/support/m2pars.f ../src/support/podred.f
../src/support/convtb.f ../src/support/m2selb.f ../src/support/podrei.f
../src/support/cputim.f ../src/support/m2selc.f ../src/support/podrgc.f
../src/support/crtptr.f ../src/support/m2seld.f ../src/support/podrgd.f
../src/support/curtim.f ../src/support/m2seli.f ../src/support/podrgi.f
../src/support/cutstr.f ../src/support/m2shll.f ../src/support/prcomf.f
../src/support/dafacu.f ../src/support/m2term.f ../src/support/prepsn.f
../src/support/dafecu.f ../src/support/m2thnq.f ../src/support/prtrap.f
../src/support/dcyphr.f ../src/support/m2time.f ../src/support/pstack.f
../src/support/dimcb_1.f   ../src/support/m2tran.f ../src/support/qlstnb.f
../src/support/dpstre.f ../src/support/m2trim.f ../src/support/qmini.f
../src/support/dspvrs.f ../src/support/m2unit.f ../src/support/qrtrim.f
../src/support/echo.f   ../src/support/m2wmch.f ../src/support/qtran.f
../src/support/edtcmd.f ../src/support/m2xist.f ../src/support/rc2grd.f
../src/support/edtcom.f ../src/support/m2year.f ../src/support/rdstmn.f
../src/support/exesys.f ../src/support/makstr.f ../src/support/rdstmt.f
../src/support/expfnm_1.f  ../src/support/matchc.f ../src/support/reglon.f
../src/support/expfnm_2.f  ../src/support/matche.f ../src/support/ressym.f
../src/support/flgrpt.f ../src/support/match.f  ../src/support/rptsym.f
../src/support/fndcmp.f ../src/support/matchm.f ../src/support/sbget_1.f
../src/support/fndntk.f ../src/support/matcho.f ../src/support/sbinit_1.f
../src/support/fndptk.f ../src/support/meta_2.f ../src/support/sbrem_1.f
../src/support/fnducv.f ../src/support/mspeld.f ../src/support/sbset_1.f
../src/support/getcml.f ../src/support/ncodec.f ../src/support/scansl.f
../src/support/getdel.f ../src/support/ncoded.f ../src/support/shosym.f
../src/support/geteq.f  ../src/support/ncodei.f ../src/support/sizecb_1.f
../src/support/getfnm_1.f  ../src/support/newfil_1.f  ../src/support/spcacb.f
../src/support/getfnm.f ../src/support/newfil.f ../src/support/spcacc.f
../src/support/getopt_1.f ../src/support/nicebt_1.f  ../src/support/stran.f
../src/support/getopt_2.f ../src/support/niceio_3.f  ../src/support/syptrc.f
../src/support/getopt.f ../src/support/nicepr_1.f  ../src/support/syptri.f
../src/support/have.f  ../src/support/no.f  ../src/support/tabrpt.f
../src/support/header.f ../src/support/nspio.f  ../src/support/trnlat.f
../src/support/iovcmp.f ../src/support/nsplgr.f ../src/support/txtops.f
../src/support/langua.f ../src/support/nspopl.f ../src/support/unitp.f
../src/support/lbdes_1.f   ../src/support/nsppwd.f ../src/support/upto.f
../src/support/lbget_1.f   ../src/support/nspsav.f ../src/support/utrans_2.f
../src/support/lbinit_1.f  ../src/support/nspxcp.f ../src/support/zzcapplt.f
../src/support/lbins_1.f   ../src/support/nthuqt.f ../src/support/zzckcvr2.f
../src/support/lbpack_1.f  ../src/support/nthuqw.f ../src/support/zzckcvr3.f
../src/support/lbrem_1.f   ../src/support/nxtcom.f ../src/support/zzckcvr4.f
../src/support/lbupd_1.f   ../src/support/occurs.f ../src/support/zzckcvr5.f
../src/support/logchk.f ../src/support/pagman.f ../src/support/zzcke06.f
../src/support/m2alph.f ../src/support/parcml.f ../src/support/zzdbrgap.f
../src/support/m2begr.f ../src/support/pltfrm.f ../src/support/zzellplt.f
../src/support/m2bodini.f  ../src/support/podaec.f ../src/support/zzellsec.f
../src/support/m2bodtrn.f  ../src/support/podaed.f ../src/support/zzgetenv.f
../src/support/m2body.f ../src/support/podaei.f ../src/support/zzgetfat.f
../src/support/m2cal.f  ../src/support/podbec.f ../src/support/zzgrdplt.f
../src/support/m2chck.f ../src/support/podbed.f ../src/support/zznsppok.f
../src/support/m2clss.f ../src/support/podbei.f ../src/support/zztxtopn.f)
target_link_libraries(support -lm spice )

endif()

if(${NIAF-TK_BUILD_STATIC_LIBRARY})

add_library(brief_f STATIC 
../src/brief/disply.f  ../src/brief/objchk.f  ../src/brief/objsbc.f
../src/brief/distim.f  ../src/brief/objcmp.f  ../src/brief/objsbf.f
../src/brief/filtem.f  ../src/brief/objfnd.f  ../src/brief/objset.f
../src/brief/getnam.f  ../src/brief/objget.f  ../src/brief/objsiz.f
../src/brief/maknam.f  ../src/brief/objinl.f  ../src/brief/objval.f
../src/brief/objact.f  ../src/brief/objmod.f  ../src/brief/prname.f
../src/brief/objadd.f  ../src/brief/objnth.f  ../src/brief/rndem.f
../src/brief/objcf1.f  ../src/brief/objnxt.f  ../src/brief/writit.f
../src/brief/objcf2.f  ../src/brief/objrem.f)
target_link_libraries(brief_f -lm )

add_library(chronos_f STATIC
	../src/chronos/crcnst.f ../src/chronos/dsplay.f ../src/chronos/lstmid.f
	../src/chronos/cronos.f ../src/chronos/ls.f ../src/chronos/speakr.f)
target_link_libraries(chronos_f -lm )

add_library(ckbref_f STATIC
	../src/ckbrief/dispsm.f  ../src/ckbrief/prinst.f  ../src/ckbrief/timecn.f
	../src/ckbrief/fixuni.f  ../src/ckbrief/repmcw.f)
target_link_libraries(ckbref_f -lm )

add_library(commnt_f STATIC ../src/commnt/clcomm.f)
target_link_libraries(commnt_f -lm )

add_library(dskbrief_f STATIC
../src/dskbrief/attcmp.f ../src/dskbrief/dski04.f  ../src/dskbrief/prcinf.f
../src/dskbrief/cortab.f ../src/dskbrief/dspdsc.f  ../src/dskbrief/sum02.f
../src/dskbrief/dskb04.f ../src/dskbrief/dspgap.f  ../src/dskbrief/sum04.f
../src/dskbrief/dskd04.f ../src/dskbrief/grpseg.f)
target_link_libraries(dskbrief_f -lm )

add_library(frmdiff_f STATIC
../src/frmdiff/chwcml.f ../src/frmdiff/dr2str.f  ../src/frmdiff/ldklst.f
../src/frmdiff/ckcovr.f ../src/frmdiff/et2str.f  ../src/frmdiff/rtdiff.f
../src/frmdiff/dpstrp.f ../src/frmdiff/getqav.f  ../src/frmdiff/sc01s2d.f)
target_link_libraries(frmdiff_f -lm )

add_library(inspekt_f STATIC
../src/inspekt/bgroup_1.f  ../src/inspekt/nspsho.f ../src/inspekt/zzhlp007.f
../src/inspekt/chunk.f  ../src/inspekt/nsptab.f ../src/inspekt/zzhlp008.f
../src/inspekt/clmgr.f  ../src/inspekt/nsptv.f  ../src/inspekt/zzhlp009.f
../src/inspekt/expand.f ../src/inspekt/nspvrb.f ../src/inspekt/zzhlp010.f
../src/inspekt/fetcha.f ../src/inspekt/other.f  ../src/inspekt/zzhlp011.f
../src/inspekt/fgroup_1.f  ../src/inspekt/param.f  ../src/inspekt/zzhlp012.f
../src/inspekt/fmtdp.f  ../src/inspekt/params.f ../src/inspekt/zzhlp013.f
../src/inspekt/fmtint.f ../src/inspekt/prep.f   ../src/inspekt/zzhlp014.f
../src/inspekt/fmttim.f ../src/inspekt/preprc.f ../src/inspekt/zzhlp015.f
../src/inspekt/hlpmen.f ../src/inspekt/proc.f   ../src/inspekt/zzhlp016.f
../src/inspekt/hlptxt.f ../src/inspekt/setchr.f ../src/inspekt/zzhlp017.f
../src/inspekt/inspkn.f ../src/inspekt/size.f   ../src/inspekt/zzhlp018.f
../src/inspekt/kerman.f ../src/inspekt/subtex.f ../src/inspekt/zzhlp019.f
../src/inspekt/list.f   ../src/inspekt/tempb.f  ../src/inspekt/zzhlp020.f
../src/inspekt/liter.f  ../src/inspekt/title.f  ../src/inspekt/zzhlp021.f
../src/inspekt/namxpn.f ../src/inspekt/tokens.f ../src/inspekt/zzhlp022.f
../src/inspekt/nspcht.f ../src/inspekt/var.f ../src/inspekt/zzhlp023.f
../src/inspekt/nspdel.f ../src/inspekt/wrtnpr.f ../src/inspekt/zzhlp024.f
../src/inspekt/nspflg.f ../src/inspekt/wrtprs.f ../src/inspekt/zzhlp025.f
../src/inspekt/nspfnd.f ../src/inspekt/zzhlp000.f  ../src/inspekt/zzhlp026.f
../src/inspekt/nspfrp.f ../src/inspekt/zzhlp001.f  ../src/inspekt/zzhlp027.f
../src/inspekt/nsphi.f  ../src/inspekt/zzhlp002.f  ../src/inspekt/zzhlp028.f
../src/inspekt/nsphlp.f ../src/inspekt/zzhlp003.f  ../src/inspekt/zzhlp029.f
../src/inspekt/nspint.f ../src/inspekt/zzhlp004.f  ../src/inspekt/zzhlp030.f
../src/inspekt/nspset.f ../src/inspekt/zzhlp005.f  ../src/inspekt/zzhlp031.f
../src/inspekt/nspshc.f ../src/inspekt/zzhlp006.f  ../src/inspekt/zzhlp032.f)
target_link_libraries(inspekt_f -lm )

add_library(mkdsk_f STATIC
../src/mkdsk/addcom.f  ../src/mkdsk/prcset.f  ../src/mkdsk/wrtdsk.f
../src/mkdsk/extmsi.f  ../src/mkdsk/prscml.f  ../src/mkdsk/zzpsxtnt.f
../src/mkdsk/makvtx.f  ../src/mkdsk/rc2cor.f  ../src/mkdsk/zztrgnvx.f
../src/mkdsk/mkgrid.f  ../src/mkdsk/rdffdi.f  ../src/mkdsk/zzvoxscl.f
../src/mkdsk/mkvarr.f  ../src/mkdsk/rdffpl.f  ../src/mkdsk/zzwseg02.f
../src/mkdsk/prcinf.f  ../src/mkdsk/rdgrd5.f)
target_link_libraries(mkdsk_f -lm )

add_library(mkspk_f STATIC
../src/mkspk/chckdo.f  ../src/mkspk/redbuf.f  ../src/mkspk/setup.f
../src/mkspk/cmlarg.f  ../src/mkspk/reorbd.f  ../src/mkspk/tle2spk.f
../src/mkspk/parsdo.f  ../src/mkspk/setelm.f)
target_link_libraries(mkspk_f -lm )

add_library(msopck_f STATIC
../src/msopck/linrot_m.f  ../src/msopck/mkfclk.f  ../src/msopck/zzmckdmp.f)
target_link_libraries(msopck_f -lm )

add_library(spacit_f STATIC
../src/spacit/ckgss.f   ../src/spacit/spab2t.f  ../src/spacit/spkwss.f
../src/spacit/ckwss.f   ../src/spacit/spalog.f  ../src/spacit/sumck.f
../src/spacit/getchr.f  ../src/spacit/spardc.f  ../src/spacit/sumek.f
../src/spacit/getint.f  ../src/spacit/spasum.f  ../src/spacit/sumpck.f
../src/spacit/pckgss.f  ../src/spacit/spat2b.f  ../src/spacit/sumspk.f
../src/spacit/pckwss.f  ../src/spacit/spkgss.f  ../src/spacit/zzconvtb.f)
target_link_libraries(spacit_f -lm )

add_library(spkdiff_f STATIC
../src/spkdiff/chwcml.f  ../src/spkdiff/getsta.f  ../src/spkdiff/stdiff.f
../src/spkdiff/dr2str.f  ../src/spkdiff/ldklst.f)
target_link_libraries(spkdiff_f -lm )

add_library(spkmerge_f STATIC
../src/spkmerge/cparse_2.f  ../src/spkmerge/rdcmd.f  ../src/spkmerge/wrdnln.f)
target_link_libraries(spkmerge_f -lm )

add_library(tobin_f STATIC
../src/tobin/zzconvtb.f)
target_link_libraries(tobin_f -lm )

endif()

add_executable(brief ../src/brief/brief.inc ../pgm_tmp/brief.f)
target_link_libraries(brief  -lm brief_f support spice )
target_include_directories(brief PRIVATE ${INCLUDE_PATH})

add_executable(chronos ../pgm_tmp/chronos.f)
target_link_libraries(chronos  -lm chronos_f support spice )
target_include_directories(chronos PRIVATE ${INCLUDE_PATH})

add_executable(commnt ../pgm_tmp/commnt.f) 
target_link_libraries(commnt  -lm commnt_f support spice )
target_include_directories(commnt PRIVATE ${INCLUDE_PATH})

add_executable(dskbrief ../pgm_tmp/dskbrief.f)
target_link_libraries(dskbrief  -lm dskbrief_f support spice )
target_include_directories(dskbrief PRIVATE ${INCLUDE_PATH})

add_executable(dskexp ../pgm_tmp/dskexp.f) 
target_link_libraries(dskexp -lm support spice )
target_include_directories(dskexp PRIVATE ${INCLUDE_PATH})

add_executable(frmdiff ../pgm_tmp/frmdiff.f) 
target_link_libraries(frmdiff -lm frmdiff_f support spice )
target_include_directories(frmdiff PRIVATE ${INCLUDE_PATH})

add_executable(inspekt ../pgm_tmp/inspekt.f)
target_link_libraries(inspekt -lm inspekt_f support spice )
target_include_directories(inspekt PRIVATE ${INCLUDE_PATH})

add_executable(mkdsk ../pgm_tmp/mkdsk.f)
target_link_libraries(mkdsk -lm mkdsk_f support spice )
target_include_directories(mkdsk PRIVATE ${INCLUDE_PATH})

add_executable(mkspk ../pgm_tmp/mkspk.f)
target_link_libraries(mkspk -lm mkspk_f support spice )
target_include_directories(mkspk PRIVATE ${INCLUDE_PATH})

add_executable(msopck ../pgm_tmp/msopck.f) 
target_link_libraries(msopck -lm msopck_f support spice )
target_include_directories(msopck PRIVATE ${INCLUDE_PATH})

add_executable(spacit ../pgm_tmp/spacit.f)
target_link_libraries(spacit -lm spacit_f support spice )
target_include_directories(spacit PRIVATE ${INCLUDE_PATH})

add_executable(spkdiff ../pgm_tmp/spkdiff.f)
target_link_libraries(spkdiff -lm spkdiff_f support spice )
target_include_directories(spkdiff PRIVATE ${INCLUDE_PATH})

add_executable(spkmerge ../pgm_tmp/spkmerge.f)
target_link_libraries(spkmerge  -lm  spkmerge_f support spice )
target_include_directories(spkmerge PRIVATE ${INCLUDE_PATH})

add_executable(tobin ../pgm_tmp/tobin.f)
target_link_libraries(tobin  -lm tobin_f support spice )
target_include_directories(tobin PRIVATE ${INCLUDE_PATH})

#add_executable(tobin ../pgm_tmp/tobin.f
#		DIRECTORY ../src/tobin_c/
#		FILES_MATCHING PATTERN "*.f" ) 
#target_link_libraries(tobin  -lm csupport cspice )

add_executable(toxfr ../pgm_tmp/toxfr.f)
target_link_libraries(toxfr -lm support spice )
target_include_directories(toxfr PRIVATE ${INCLUDE_PATH})

add_executable(version ../pgm_tmp/version.f)
target_link_libraries(version -lm support spice )
target_include_directories(version PRIVATE ${INCLUDE_PATH})

if(BUILD_COOKBOOK)

add_executable(simple ../pgm_tmp/simple.f)
target_link_libraries(simple -lm support spice )
target_include_directories(simple PRIVATE ${INCLUDE_PATH})

add_executable(states ../pgm_tmp/states.f)
target_link_libraries(states  -lm support spice )
target_include_directories(states PRIVATE ${INCLUDE_PATH})

add_executable(subpt ../pgm_tmp/subpt.f)
target_link_libraries(subpt -lm support spice )
target_include_directories(subpt PRIVATE ${INCLUDE_PATH})

add_executable(tictoc ../pgm_tmp/tictoc.f)
target_link_libraries(tictoc -lm support spice )
target_include_directories(tictoc PRIVATE ${INCLUDE_PATH})

endif()
# last remove pgm_tmp
# file(REMOVE_RECURSE ${PROJECT_SOURCE_DIR}/pgm_tmp/)
# OK this doesn't work for now it removes it before build WTF???

install(TARGETS spice support
 	DESTINATION lib/)
install(TARGETS brief chronos commnt dskbrief dskexp frmdiff
		inspekt msopck spacit spkdiff spkmerge tobin toxfr version  
 	DESTINATION bin/)
INSTALL (DIRECTORY ${INCLUDE_PATH} 
	DESTINATION include/naif_toolkit
	FILES_MATCHING 
	PATTERN "*.inc")
INSTALL (DIRECTORY ${INCLUDE_PATH} DESTINATION share/naif_toolkit
	FILES_MATCHING
	PATTERN "*.f" 
	PATTERN "*.pgm")
INSTALL (DIRECTORY ../data 
	DESTINATION share/naif_toolkit/data)
INSTALL (DIRECTORY ../doc 
	DESTINATION share/naif_toolkit/doc)

