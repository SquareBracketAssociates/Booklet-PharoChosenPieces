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




### Storing a Form from a PNG

Going over the web to grab a png may not be satisfactory (for example if you do not have internet). 
We propose you to store the png as a Form in a method body. So that we can just get a png by invoking a method that will 
create the form outside of a serialized representation. 

Browse the class `PNGReadWriter` you can see the method `formFromStream`

```
ImageReadWriter formFromStream: 'test.png' asFileReference binaryReadStream
```

The following snippet read the contents of a PNG file and produces a Pharo expression that once executed
will create a form that represents the PNG.

```
| str |
str := String streamContents: [ :str |
         (PNGReadWriter formFromFileNamed: 'PNG.png') storeOn: str ].

"compiling a new 
MyClass compile: 'pngContents ^ ', str contents classified: 'flags'

MyClass new pngContents inspect
```

### Serializing a PNG


```

```
