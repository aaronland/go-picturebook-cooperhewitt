# go-picturebook-cooperhewitt

![](docs/images/picturebook-cooperhewitt-pdf.png)

Create a PDF file (a "picturebook") from a folder containing images, with support for images exported from the Cooper Hewitt API.

## Tools

To build binary versions of these tools run the `cli` Makefile target. For example:

```
$> make cli
go build -mod vendor -o bin/picturebook cmd/picturebook/main.go
```

### picturebook

Create a PDF file (a "picturebook") from a folder containing images, with support for images exported from the Cooper Hewitt API.

```
> ./bin/picturebook -h
  -border float
    	The size of the border around images. (default 0.01)
  -caption string
    	A valid caption.Caption URI. Valid schemes are: cooperhewitt, filename, none, orthis
  -debug
    	DEPRECATED: Please use the -verbose flag instead.
  -dpi float
    	The DPI (dots per inch) resolution for your picturebook. (default 150)
  -exclude value
    	A valid regular expression to use for testing whether a file should be excluded from your picturebook. DEPRECATED: Please use -filter regexp://exclude/?pattern={REGULAR_EXPRESSION} flag instead.
  -filename string
    	The filename (path) for your picturebook. (default "picturebook.pdf")
  -fill-page
    	If necessary rotate image 90 degrees to use the most available page space.
  -filter value
    	A valid filter.Filter URI. Valid schemes are: any, cooperhewitt, orthis, regexp
  -height float
    	A custom width to use as the size of your picturebook. Units are currently defined in inches. This fs.overrides the -size fs. (default 11)
  -include value
    	A valid regular expression to use for testing whether a file should be included in your picturebook. DEPRECATED: Please use -filter regexp://include/?pattern={REGULAR_EXPRESSION} flag instead.
  -ocra-font
    	Use an OCR-compatible font for captions.
  -orientation string
    	The orientation of your picturebook. Valid orientations are: 'P' and 'L' for portrait and landscape mode respectively. (default "P")
  -pre-process value
    	DEPRECATED: Please use -process {PROCESS_NAME}:// flag instead.
  -process value
    	A valid process.Process URI. Valid schemes are: halftone, null, rotate
  -size string
    	A common paper size to use for the size of your picturebook. Valid sizes are: [please write me] (default "letter")
  -sort string
    	A valid sort.Sorter URI. Valid schemes are: modtime, orthis
  -source-uri string
    	A valid GoCloud blob URI to specify where files should be read from. By default file:// URIs are supported.
  -target string
    	Valid targets are: cooperhewitt; orthis. If defined this flag will set the -filter and -caption flags accordingly. DEPRECATED: Please use specific -filter and -caption flags as needed.
  -target-uri string
    	A valid GoCloud blob URI to specify where your final picturebook PDF file should be written to. By default file:// URIs are supported.
  -verbose
    	Display verbose output as the picturebook is created.
  -width float
    	A custom height to use as the size of your picturebook. Units are currently defined in inches. This fs.overrides the -size fs. (default 8.5)
```

### Notes

The `picturebook` tool does not download photos from Cooper Hewitt. That is left to another process to do using the [Cooper Hewitt API](https://collection.cooperhewitt.org/api/) and specifically the [cooperhewitt.shoebox.items](https://collection.cooperhewitt.org/api/methods/#cooperhewitt.shoebox.items) methods. 

It also makes the following assumptions about file names and file structures:

* That the results of the [cooperhewitt.shoebox.items.getInfo](https://collection.cooperhewitt.org/api/methods/cooperhewitt.shoebox.items.getInfo) API method have been captured and stored in a file titled `{SHOEBOX_ID}.json`.
* The original photo or image has been downloaded and stored in the same directory as the `{SHOEBOX_ID}.json` file. This is the image with the `_b.jpg` suffix.


For example:

```
$> ls -al cooperhewitt/104/905/9
-rw-r--r--   1 user  staff     6272 Nov 29  2017 18649107.json
-rw-r--r--   1 user  staff  1329530 Nov 29  2017 89859_be1ef14e2e459ec9_b.jpg
```

## Handlers

The `picturebook` application supports a number of "handlers" for customizing which images are included, how and whether they are transformed before inclusion and how to derive that image's caption.

In addition to the default `picturebook` handlers this package exports the following:

### Captions

```
type Caption interface {
	Text(context.Context, *blob.Bucket, string) (string, error)
}
```

#### cooperhewitt://

This handler will derive the title for a Cooper Hewitt collection object using data stored in a `index.json` file, alongside an image. The data in the file is expected to be the out of a call to the [cooperhewitt.shoebox.items.getInfo](https://collection.cooperhewitt.org/api/methods/cooperhewitt.shoebox.items.getInfo) API method.

### Filters

```
type Filter interface {
	Continue(context.Context, *blob.Bucket, string) (bool, error)
}
```

#### cooperhewitt://

This handler will ensure that only images whose filename matches `_b.jpg$` and that have a sibling `index.json` file are included.

## See also

* https://github.com/aaronland/go-picturebook
* https://collection.cooperhewitt.org/api/