# Blue Foot: Image Sizes
## Existing Images
Out of the box 3 images sizes exist within BlueFoot:

- thumbnail (300 x 200)
- medium (860 x 450)
- large (1400 x 750)

These can all be found in app/code/community/Gene/BlueFoot/etc/bluefoot/bluefoot.xml


### Using content app image sizes
To use BlueFoots image sizes in a phtml template you should use our image helper
```
<?php
$_entity = $this->getEntity();
$_imageHelper = Mage::helper('gene_bluefoot/image');
?>

<?php if(($_image = $_entity->getFeaturedImage())): ?>
    <img src="<?php echo $_imageHelper->init($_image)->useConfig('medium'); ?>" alt="<?php echo $_entity->getTitle(); ?>"/>
<?php endif; ?>
```
In this example you can see the 'medium' image is used.

### Add custom image sizes
Adding custom image sizes is simple.

First you must add your own bluefoot.xml to your module (app/code/local/NAMESPACE/MODULE/etc/bluefoot/bluefoot.xml)

Then add the following code:

```
<?xml version="1.0"?>
<config>
    <frontend>
        <image_profiles>
            <IMAGE_ALIAS>
                <width>WIDTH</width>
                <height>HEIGHT</height>
            </IMAGE_ALIAS>
        </image_profiles>
    </frontend>
</config>
```
In this example replace 'IMAGE_ALIAS' with your alias and the width and height you need.

### Custom image options
You can use pass the following options to your image

```
<width></width>
<height></height>
<adaptive_resize></adaptive_resize>
<quality></quality>
<keep_aspect_ratio></keep_aspect_ratio>
<constrain_only></constrain_only>
```
