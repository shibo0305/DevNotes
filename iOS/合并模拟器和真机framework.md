模拟器：iphone4s-5 : i386 ， iphone5s-6plus : x86_64。
真机：iphone3gs-4s : armv7 ， iphone5-5c : armv7s （静态库只要支持了armv7，就可以跑在armv7s的架构上）， iphone5s-6plus : arm64。

armv6, armv7, armv7s是ARM CPU的不同指令集，原则是向下兼容的。例如iPhone4S CPU支持armv7, 但它同时兼容armv6，只是使用armv6指令可能无法充分发挥它的特性。

在Build Phases中增加Run Script：

```
if [ "${ACTION}" = "build" ]
then
INSTALL_DIR=${SRCROOT}/Products/${PROJECT_NAME}.framework
 
DEVICE_DIR=${BUILD_ROOT}/${CONFIGURATION}-iphoneos/${PROJECT_NAME}.framework
 
SIMULATOR_DIR=${BUILD_ROOT}/${CONFIGURATION}-iphonesimulator/${PROJECT_NAME}.framework
 
 
if [ -d "${INSTALL_DIR}" ]
then
rm -rf "${INSTALL_DIR}"
fi
 
mkdir -p "${INSTALL_DIR}"
 
cp -R "${DEVICE_DIR}/" "${INSTALL_DIR}/"
#ditto "${DEVICE_DIR}/Headers" "${INSTALL_DIR}/Headers"
 
lipo -create "${DEVICE_DIR}/${PROJECT_NAME}" "${SIMULATOR_DIR}/${PROJECT_NAME}" -output "${INSTALL_DIR}/${PROJECT_NAME}"
 
#open "${DEVICE_DIR}"
#open "${SRCROOT}/Products"
fi
```

