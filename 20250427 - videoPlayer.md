20250427

今天修了一个video player的bug
Video player应该是不是一种UI element
如果是canvas内部的话，可以在canvas scaler中调整match，match with width or height
video player自带aspect ratio,fit inside work with carmera rendering but not in the render texture case
texture case involve with raw image as the texture, but raw image will go with rect transform (stretch or default size)
if go with stretch, then fit inside won't work
网上找到了类似手动更改raw image长款的方法

https://discussions.unity.com/t/rawimage-with-correct-aspect-ratio/229092/3

int[] scaleResolution(int width, int heigth, int maxWidth, int maxHeight)
{
    int new_width = width;
    int new_height = heigth;
    
    if (width > maxWidth){//方框
        new_width = maxWidth;
        new_height = (new_width * heigth) / width;
    }
    else//长框
    {
        new_height = maxHeight;
        new_width = (new_height * width) / heigth;
    }
    int[] dimension = { new_width, new_height };
    return dimension;
} 

改了一下这样的感觉就ok
