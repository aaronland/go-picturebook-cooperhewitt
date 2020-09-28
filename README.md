# go-picturebook-cooperhewitt

Create a PDF file (a "picturebook") from a folder containing images, with support for images exported from the Cooper Hewitt API.

## Important

Work in progress. Documentation to follow.

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

_TBW_

## See also

* https://github.com/aaronland/go-picturebook
* https://collection.cooperhewitt.org/api/