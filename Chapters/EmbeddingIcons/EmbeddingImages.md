## Embedding image

You may want to manage icons without dealing with external files for example when you use a memory file system in your tests.
You can store the png binary files in a string returned by a method. 
This requires, however some care such as using base64 encoder or decoder.


### From uuencoded to bitmap

Giving a uuencoded string that represents the bitmap of a png, you can get back a form (the bitmap physical representation)
using the following expression: `Form fromBinaryStream: uu base64Decoded asByteArray readStream`.

```
| uu |
uu := 'iVBORw0KGgoAAAA...'.

Form fromBinaryStream: uu base64Decoded asByteArray readStream
```




### Some bricks for meta programming

Going over the web to grab the flag png may not be satisfactory (for example if you do not have internet). 
We propose you to store the png as method bodies. So that we can just get a png by invoking a method that will 
create the png outside of a serialized representation. 

Here are some basic bricks for this exercise that is basically a first simple meta programming approach. 

Imagine that we have a png file and that we want to store as a method body so that we can get it without the need to have it on disc.
To achieve this we can simple compile a new method with as body a way to create a png. 

Browse the class `PNGReadWriter` you can see the method formFromStream

ImageReadWriter formFromStream: 'test.png' asFileReference binaryReadStream

```
| str |
str := String streamContents: [ :str |
         (PNGReadWriter formFromFileNamed: 'CardBack.png') storeOn: str ].

"compiling a new 
Country compile: 'uaFlag ^ ', str contents classified: 'flags'

Country new uaFlag inspect
```



