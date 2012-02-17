SpraedPDFGeneratorBundle
===============

SpraedPDFGeneratorBundle generates XHTML documents to PDF including header and footer.
It works with a little jar library based on the [Flying Saucer project][flyingsaucer]

Installation
------------

*If your are using git and manages your vendors as submodules, use the following commands to add this bundle to your Symfony project.*

Copy the SpraedPDFGeneratorBundle into the `vendor/bundles/Spraed/PDFGeneratorBundle` directory:

    git submodule add https://github.com/stedekay/SpraedPDFGeneratorBundle.git vendor/bundles/Spraed/PDFGeneratorBundle

Or use deps file:

    [SpraedPDFGeneratorBundle]
        git=https://github.com/stedekay/SpraedPDFGeneratorBundle.git
        target=/bundles/Spraed/PDFGeneratorBundle

Register the source directory in your autoloader:

    $loader->registerNamespaces(array(
        ...
        'Spraed'                        => __DIR__.'/../vendor/bundles',

Finally, you can enable it in your kernel:

    // app/AppKernel.php
    public function registerBundles()
    {
        $bundles = array(
            ...
            new Spraed\PDFGeneratorBundle\SpraedPDFGeneratorBundle(),
            ...

Usage
-----

There is a service registered in the services.yml to generate pdf files.
Just call the PDF generator from the service class and call the generatePDF()-method
with the XHTML and the url of the PDF:

	$html = $this->renderView('AcmeDemoBundle:Default:index.html.twig');
	$pdfGenerator = $this->get('spraed.pdf.generator');

Also you are able to set an encoding option.

	$pdfGenerator->generatePDF($html, 'UTF-8');

Anything else will be handled by the "new Response()" object in the controller, i.e.:

        $html = $this->renderView('ACMEYourBundle:Print:print.html.twig');
        $pdfGenerator = $this->get('spraed.pdf.generator');

        return new Response($pdfGenerator->generatePDF($html, 'UTF-8'),
                        200,
                        array(
                            'Content-Type' => 'application/pdf',
                            'Content-Disposition' => 'inline; filename="out.pdf"'
                        )
        );

If you wish the pdf to be offered as a download, simply change 'inline' in 'Content-Disposition' to 'attachment';

To define proper print css you might want to read into the w3.org's hints on that: [w3.org]
[w3.org]: http://www.w3.org/TR/css3-page/
[flyingsaucer]: http://code.google.com/p/flying-saucer/
[spraed]: http://www.spraed.com