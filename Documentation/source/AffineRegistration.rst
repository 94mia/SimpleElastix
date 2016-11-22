Affine Registration
===================

The affine transform allows for shearing and scaling in addition to the rotation and translation we saw in the previous section. For example, if we are interested in registering bones from different patients, or want to initialize a non-rigid registration, the affine transform is usually a good choice. For example, if we are interested in registering bones from different patients, or want to initialize a non-rigid registration, the affine transform is usually a good choice. The affine transform is selected using :code:`(Transform "AdvancedAffineTransform")`.

Consider the images in Figure 10.

.. _fig10: 

    .. image::  _static/BrainProtonDensity.png
       :width: 45%
       :align: left
    .. image::  _static/BrainProtonDensityTranslatedR1013x17yS12.png
       :width: 45%
       :align: left

    .. class:  center
    
    Figure 10. The original image :code:`fixedImage.nii` (left) and translated and rotated image :code:`movingImage.nii` (right).

The image on the right has been sheared, scaled 1.2x, rotated 10 degrees and translated 13 pixels in the x-direction and 17 pixels in the y-direction. Using the :code:`AdvancedAffineTransform` we may correct for this misalignment.

::

    import SimpleITK as sitk

    SimpleElastix = sitk.SimpleElastix()
    SimpleElastix.SetFixedImage(sitk.ReadImage("fixedImage.nii")
    SimpleElastix.SetMovingImage(sitk.ReadImage(movingImage.nii")
    SimpleElastix.SetParameterMap(sitk.GetDefaultParameterMap("affine"))
    SimpleElastix.Execute()
    sitk.WriteImage(SimpleElastix.GetResultImage())

It is clear from the result mean image on right in Fig. 11 that registration was successful.

.. _fig11: 

    .. image::  _static/PreAffine.jpeg
       :width: 45%
       :align: left
    .. image::  _static/PostAffine.jpeg
       :width: 45%
       :align: left

    .. class:  center
    
    Figure 11. Mean image before registration (left) and mean image after registration (right).

Notice that the only difference from the previous example is the requested parameter map. In fact, the following code will use the same registraiton method as we saw in the prevous section.:

::

    import SimpleITK as sitk

    SimpleElastix = sitk.SimpleElastix()
    SimpleElastix.SetFixedImage(sitk.ReadImage("fixedImage.nii")
    SimpleElastix.SetMovingImage(sitk.ReadImage(movingImage.nii")
    parameterMap = sitk.GetDefaultParameterMap("rigid")
    parameterMap["Transform"] = ["AffineTransform"]
    SimpleElastix.SetParameterMap()
    SimpleElastix.Execute()
    sitk.WriteImage(SimpleElastix.GetResultImage())

This demonstrates how easy it is to try out different registration components with SimpleElastix. In the next example we will introduce non-rigid registration and initialize the moving image with an affine transform.