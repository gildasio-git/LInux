<h3>Utilitário de linha de comando para reduzir tamanho de arquivo PDF via comando</h3>

* USANDO O GHOSTSCRIPT

`sudo apt install ghostscript`

* Você pode usar o comando mágico para compactar PDFs para uma qualidade Legível

`gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/screen -dNOPAUSE -dQUIET -dBATCH -sOutputFile=output.pdf input.pdf
`

* Os vários ajustes na opção -dPDFSETTINGS são fornecidos na tabela abaixo. Use-os de acordo com a sua necessidade.

dPDFSETTINGS | DESCRIÇÃO
------------------------
-dPDFSETTINGS=/screen|Has a lower quality and smaller size. (72 dpi)
-dPDFSETTINGS=/ebook|Has a better quality, but has a slightly larger size (150 dpi)
-dPDFSETTINGS=/prepress	|Output is of a higher size and quality (300 dpi)
-dPDFSETTINGS=/printer | Output is of a printer type quality (300 dpi)
-dPDFSETTINGS=/default | Selects the output which is useful for multiple purposes. Can cause large PDFS.

* uSE PS2PDF

>Este comando ps2pdf converte um PDF em PS e depois novamente, compactando-o com eficiência como resultado.Pode nem sempre funcionar, mas pode dar resultados muito bons. Formatar: ps2pdf input.pdf output.pdf
É recomendável usar a configuração -dPDFSETTINGS=/ebooks para obter o melhor desempenho, pois os ebooks têm o melhor tamanho para facilitar a leitura e também são pequenos o suficiente.

`ps2pdf -dPDFSETTINGS=/ebook input.pdf output.pdf`




