### camerainfo
```
*camerainfo <range>,<rotation>,<latitude>{,<char id>};
```

This command will update the client's camera information with the given values where
the client can be the attached character or the player given by the char id parameter.
Note: This requires 2016-05-25aRagexeRE or newer.

The values given will be divided by 100 and transmitted as floating-point number.

```
	range		The zoomfactor of the camera.
				Default: 230000 (230.0) when fully zoomed in
				Maximum: 400000 (400.0) when fully zoomed out

	rotation	The rotation of the camera.
				Default: 0 (0.0) when no rotation is applied
				Maximum: 360000 (360.0°) when fully rotated

	latitude	The angle of the camera.
				Default: -50000 (-50.0)
				Maximum: -75000 (-75.0)
```
