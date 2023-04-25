
```cmake
get_target_property(_qmake_executable Qt${QT_VERSION_MAJOR}::qmake IMPORTED_LOCATION)  
get_filename_component(_qt_bin_dir "${_qmake_executable}" DIRECTORY)
find_program(DEPLOYQT_EXECUTABLE windeployqt HINTS "${_qt_bin_dir}")
add_custom_command(TARGET ${PROJECT_NAME}  
	POST_BUILD
	# Deploy the Qt Application  
	COMMAND ${DEPLOYQT_EXECUTABLE}  
	# --no-translations  
	--no-compiler-runtime
	$<TARGET_FILE:${PROJECT_NAME}>  
)
```
+ no-translations  即不要翻译文件
+ no-compiler-runtime 即不要运行时动态库