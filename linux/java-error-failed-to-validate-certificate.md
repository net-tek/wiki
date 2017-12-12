<!-- TITLE: Java Error: Failed to validate certificate. The application will not be executed -->

You can disable this check, because you have start the applet to access your FC Switch. Locate the file java.security in the `lib/security` folder of your java installation and comment the following:

`jdk.certpath.disabledAlgorithms=MD2, RSA keySize < 1024`

The applet should start now but for security reasons it is recommended to reverse this change if it is no longer needed.