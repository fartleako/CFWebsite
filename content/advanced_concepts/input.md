---
title: "Handling Input"
date: 2022-11-01T21:49:43+01:00
draft: false
weight: 10
---

The classes **Keyboard** and **Mouse** represent - as the names indicate - your basic input devices. They are part of the window and have all the standard keys and actions listed, while constantly checking their state which is received from the system automatically. Through the functions **Keyboard::keyPressed(Key)** and **Mouse::buttonState(Button)** you can check if the Key/Button is pressed.

You can use it like this:
```cpp
//GLWindow pRenderWin;

if (pRenderWin.keyboard()->keyPressed(Keyboard::KEY_F9, true)) {
    function.toggle();                                                          //just an example
}
```

The second argument is reset: setting it true will only let you toggle a function while being set to false (by default) you can also hold the key, which can be used for movement keys for example.
```cpp
if (pRenderWin.keyboard()->keyPressed(Keyboard::KEY_W)) {
    camera.moveForward(0.1f);                                                   //just an example
}
```

Aside from mouse buttons, you can also track mouse movement:
```cpp
bool m_CameraRotation = false;
Mouse pMouse = pRenderWin.mouse();
//VirtualCamera pCamera;

if(pMouse->buttonState(Mouse::BTN_RIGHT)) {
    if (m_CameraRotation) {
        const Eigen::Vector2f MouseDelta = pMouse->movement();
        pCamera->rotY(CForgeMath::degToRad(-0.1f * MouseDelta.x()));
        pCamera->pitch(CForgeMath::degToRad(-0.1f * MouseDelta.y()));
            
        }
    else {
        m_CameraRotation = true;  
    }
    pMouse->movement(Eigen::Vector2f::Zero());
	}
else {
    m_CameraRotation = false;
}
```
This would rotate your camera according to your mouse movement while pressing the right mouse button.

{{% notice note %}}
As you can see, while **Keyboard** and **Mouse** are seperate classes, they are also part of **GLWindow**, so to access the keyboard and mouse of the system, you have to access it via the window.
{{% /notice %}}