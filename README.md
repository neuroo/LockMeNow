# LockMeNow!

My iPhone 5 lock button is not working properly, so I wrote this application.

## Locking the iDevice

Since I want to lock the device, I need to be able to trigger the same functionality iOS does when the lock button is hit.
However, Apple does not play nice with its private frameworks. After some research, I found that I need to call the `GSEventLockDevice()` function which belongs to a private framework `GraphicsServices`. 

The approach I take with this application is to dynamically load the lib and call this function. This is easily done using the `dlopen` and `dlsym` functions.

The code looks like this:
```objective-c
char *gsDylib = "/System/Library/PrivateFrameworks/GraphicsServices.framework/GraphicsServices";
void *handle = dlopen(gsDylib, RTLD_NOW);
if (handle) {
  BOOL locked = FALSE;
  void (*_GSEventLockDevice)() = dlsym(handle, "GSEventLockDevice");
  if (_GSEventLockDevice)  {
    _GSEventLockDevice();
    //...
  }
  dlclose(handle);
  //...
}
```

# Limitations
This application can obviously not be accepted (as is) on the Apple Store, and that's mostly why I put it here. If you want to use the application, you will need a developer cert (or a jailbroken device and using the iDevice build toolchain).

# Author
You can find me on twitter: [@rgaucher](https://twitter.com/rgaucher)