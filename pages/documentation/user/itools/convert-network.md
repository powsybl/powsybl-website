---
layout: default
---

# iTools convert-network

The `convert-network` command is used to convert a grid file from a format to another. The input format is automatically detected, whereas the output format must be specified.

## Usage
```
$> itools convert-network --help
usage: itools [OPTIONS] convert-network [-E <property=value>]
       [--export-parameters <EXPORT_PARAMETERS>] [--help] [-I <property=value>]
       [--import-parameters <IMPORT_PARAMETERS>] --input-file <INPUT_FILE>
       --output-file <OUTPUT_FILE> --output-format <OUTPUT_FORMAT>

Available options are:
    --config-name <CONFIG_NAME>   Override configuration file name

Available arguments are:
 -E <property=value>                          use value for given exporter
                                              parameter
    --export-parameters <EXPORT_PARAMETERS>   the exporter configuration file
    --help                                    display the help and quit
 -I <property=value>                          use value for given importer
                                              parameter
    --import-parameters <IMPORT_PARAMETERS>   the importer configuation file
    --input-file <INPUT_FILE>                 the input file
    --output-file <OUTPUT_FILE>               the output file
    --output-format <OUTPUT_FORMAT>           the output file format

Where OUTPUT_FORMAT is one of [CGMES, AMPL, UCTE, XIIDM]
```

### Required arguments

**\-\-input-file**  
This option defines the path of the input file. The [supported formats](../../index.html#grid-formats) depend on the execution class path.

**\-\-output-file**  
This option defines the path of the output file.

**\-\-output-format**  
This option defines the format of the output file. The list of [supported formats](../../index.html#grid-formats) are listed between brackets in the command help.

### Optional arguments

**\-\-export-parameters**  
This option defines the path of the [exporter](../../glossary.md#exporter)'s configuration file. It's possible to overload one or many parameters using the `-E property=value` syntax. The list of supported properties depends on the [output format](../../index.html#grid-formats).

**\-\-import-parameters**  
This option defines the path of the [importer](../../glossary.md#importer)'s configuration file. It's possible to overload one or many parameters using the `-I property=value` syntax. The list of supported properties depends on the [input format](../../index.html#grid-formats).

## Examples

This example shows how to convert a [UCTE-DEF](../../grid/formats/ucte-def.md) file to an [XIIDM](../../grid/formats/xiidm.md) file:
```
$> itools convert-network --input-file case-file.uct --output-format XIIDM --output-file case-file.xiidm
```

This example shows how to pass an exporter's configuration file, and overload one of the properties:
```
$> itools convert-network --input-file case-file.uct --output-format XIIDM --output-file case-file.xiidm --export-parameters xiidm.properties -E iidm.export.xml.indent=false
```
