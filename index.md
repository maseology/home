---
title:  "index page for testing"
author: Mason Marchildon
date:   2022-02-09
output: html_document
knit:   (
            function(input_file, encoding) {
                out_dir <- '';
                rmarkdown::render(
                    input_file,
                    encoding=encoding,
                    output_file=file.path(dirname(input_file), out_dir,
                    'index.html')
                )
            }
        )
---

## index page

* **[Barometry](/home/interp/baro.html)**
