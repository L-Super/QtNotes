在 `CMakeLists.txt` 文件中，加入以下语句，即可实现，编译时，自动将 Qt 相关依赖动态库拷贝到可执行文件路径下。
```cmake
get_target_property(QMakeExecutable Qt${QT_VERSION_MAJOR}::qmake IMPORTED_LOCATION)  
get_filename_component(QtBinDir "${QMakeExecutable}" DIRECTORY)  
find_program(DEPLOY_QT_EXECUTABLE windeployqt macdeployqt HINTS "${QtBinDir}")

# Deploy the Qt Application
if (WIN32)
    add_custom_command(TARGET ${PROJECT_NAME}
            POST_BUILD
            COMMAND ${DEPLOY_QT_EXECUTABLE}
            --no-translations
            # --no-compiler-runtime
            $<TARGET_FILE:${PROJECT_NAME}>
    )
elseif (APPLE)
    add_custom_command(TARGET ${PROJECT_NAME}
            POST_BUILD
            COMMAND ${DEPLOY_QT_EXECUTABLE}
            "$<TARGET_BUNDLE_DIR:${PROJECT_NAME}>"
            # -dmg
    )
endif ()
```
+ no-translations  即不要翻译文件
+ no-compiler-runtime 即不要运行时动态库