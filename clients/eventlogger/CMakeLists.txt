find_package(Qt4)

if (QT4_FOUND)
	include(${QT_USE_FILE})
	
	find_path(QWT_INCLUDE_DIR qwt_plot.h /usr/include/qwt-qt4 /usr/local/include/qwt-qt4)
	find_library(QWT_LIBRARIES qwt /usr/lib /usr/local/lib)
	if (NOT QWT_LIBRARIES)
		find_library(QWT_LIBRARIES qwt-qt4 /usr/lib /usr/local/lib)
	endif (NOT QWT_LIBRARIES)
	
	if (QWT_INCLUDE_DIR AND QWT_LIBRARIES)
	
		set(eventlogger_SRCS eventlogger.cpp)
		
		include_directories (${QWT_INCLUDE_DIR} ${QT_INCLUDE_DIR} ${CMAKE_BINARY_DIR}/eventlogger)
		
		#add_executable(asebastudio WIN32 ${eventlogger_SRCS} ${resfiles})
		add_executable(asebaeventlogger WIN32 ${eventlogger_SRCS})
		
		# Only include the qtmain lib if we are on the windows platform
		if (WIN32)
			target_link_libraries(asebaeventlogger ${QWT_LIBRARIES} ${QT_LIBRARIES} ${QT_QTMAIN_LIBRARY}  ${ASEBA_CORE_LIBRARIES})
		else (WIN32)
			target_link_libraries(asebaeventlogger ${QWT_LIBRARIES} ${QT_LIBRARIES}  ${ASEBA_CORE_LIBRARIES})
		endif (WIN32)
	
	else (QWT_INCLUDE_DIR AND QWT_LIBRARIES)
	
		message("Cannot find Qwt")
		
	endif (QWT_INCLUDE_DIR AND QWT_LIBRARIES)
	
endif (QT4_FOUND)