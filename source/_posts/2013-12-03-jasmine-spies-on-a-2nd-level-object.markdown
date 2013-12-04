---
layout: post
title: "Jasmine Spies on a 2nd level object"
date: 2013-12-03 16:03
comments: true
categories: [[jasmine, angularjs]]
---
I've been trying to mock a 2 level object that looks like this:

```
        mockSoundService = {
            getSound: function () {
                return {
                    volume: function () {
                    }
                };
            }
        };
```
Naturally, I thought it would work with something like this in my test:
```
        spyOn(mockSoundService.getSound(), 'volume');
        service.closeIntroVideo();

        expect(service.getPlayer()).toBeNull();
        expect(mockSoundService.getSound().volume).toHaveBeenCalled();
```

However, I kept getting errors like these:

```
Expected a spy, but got undefined.
```

It turns out that you need to break apart the 2nd level object that was nested. Therefore the final product should look something like this:

```
        mockSound = {
            volume: function () {
            }
        };

        mockSoundService = {
            getSound: function () {
                return mockSound;
            }
        }
```

and now in your mocks you can do something like this:

```
        spyOn(mockSound, 'volume');
        spyOn(mockState, 'go');
        service.closeIntroVideo();
```
