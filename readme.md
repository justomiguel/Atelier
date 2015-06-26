# Atelier
*Every artist needs a place where he can paint in peace and store his tools such as palette and swatches.*

![](https://img.shields.io/github/release/Musenkishi/Atelier.svg?label=JitPack%20Maven)

This is a library aiming to simplify the use of Palette, especially in lists such as ListView, GridView, or RecyclerView.

![](assets/sample.gif)

## Download

Add the [Jitpack][jit] repository to your project-level or module-level `build.gradle`:
```groovy
repositories {
	maven {
		url "https://jitpack.io"
    }
}
```

Now, you can add the Atelier dependency:
```groovy
compile 'com.github.Musenkishi:Atelier:1.3.0'
```

## Usage

Currently Atelier supports classes (and extensions of):
* View
* ImageView (masking and background)
* TextView
* Floating Action Button (icon masking and background)
* CardView

If the supplied view is or extends `View`, Atelier will apply the color to the background. If view extends `ImageView` and you use `.mask()`, Atelier will apply the color to the image, not the background. This is also true for Floating Action Button. And if view is, or extends `TextView`, the color will be applied to the text, not the background. If view is or extends `CardView`, Atelier will apply the color through `.setCardBackgroundColor()`.

```java
Atelier.with(context, uniqueStringForBitmap) //For reuse purposes uniqueStringForBitmap could be the url for the image.
        .load(bitmap)
        .swatch(new DarkVibrantSwatch(ColorType.BACKGROUND))
        .into(view, view2, ...); //Single or multiple views supported
```

## Example
In this example [Glide][glide] is used, which is similar to Picasso:
```java
RequestListener<String, GlideDrawable> glideDrawableRequestListener = new RequestListener<String, GlideDrawable>() {
    @Override
    public boolean onResourceReady(GlideDrawable resource,
                                   String model,
                                   Target<GlideDrawable> target,
                                   boolean isFromMemoryCache,
                                   boolean isFirstResource) {
        Bitmap bitmap = ((GlideBitmapDrawable) resource).getBitmap();
        if (bitmap != null) {
            Context context = viewHolder.bottomBar.getContext();

		    //Set color to a View's background
            Atelier.with(context, model)
                        .load(bitmap)
                        .swatch(new VibrantSwatch(ColorType.BACKGROUND))
                        .into(viewHolder.bottomBar);

		    //Set color to text in a TextView
            Atelier.with(context, model)
                        .load(bitmap)
                        .swatch(new VibrantSwatch(ColorType.TEXT_TITLE))
                        .into(viewHolder.textViewResolution);

		    //Colorize an ImageView/ImageButton with .mask().
            //Best result is accomplished with a white image (transparent bakground).
            Atelier.with(context, model)
                        .load(bitmap)
                        .fallbackColor(viewHolder.textViewResolution.getCurrentTextColor())
                        .swatch(new VibrantSwatch(ColorType.TEXT_TITLE))
                        .mask()
                        .into(viewHolder.imageButton);
            }
            return false;
        }
    };

    Glide.with(viewHolder.bottomBar.getContext())
            .load(imageUrl)
            .fitCenter()
            .placeholder(R.color.Transparent)
            .listener(glideDrawableRequestListener)
            .into(viewHolder.imageView);
```

Atelier will generate the palette in a seperate thread (up to 5 threads on post-lollipop) and apply the color in the assigned view.

## License

	Copyright 2015 Freddie Lust-Hed

	Licensed under the Apache License, Version 2.0 (the "License");
	you may not use this file except in compliance with the License.
	You may obtain a copy of the License at

	   http://www.apache.org/licenses/LICENSE-2.0

	Unless required by applicable law or agreed to in writing, software
	distributed under the License is distributed on an "AS IS" BASIS,
	WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
	See the License for the specific language governing permissions and
	limitations under the License.


 [jmdns]: https://github.com/openhab/jmdns
 [jit]: https://jitpack.io
 [glide]: https://github.com/bumptech/glide
