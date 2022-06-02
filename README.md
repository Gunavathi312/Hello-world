int width = 100;
int height = 100;
String filetype = "png";
com.google.zxing.qrcode.QRCodeWriter writer = new com.google.zxing.qrcode.QRCodeWriter();
com.google.zxing.common.BitMatrix matrix = null;
try {
matrix = writer.encode(text, com.google.zxing.BarcodeFormat.QR_CODE, width, height);
}
catch (com.google.zxing.WriterException e) { throw new PRRuntimeException(e); }
java.awt.image.BufferedImage image = new java.awt.image.BufferedImage(width, height, java.awt.image.BufferedImage.TYPE_INT_RGB);
image.createGraphics();
java.awt.Graphics2D graphics = (java.awt.Graphics2D) image.getGraphics();
graphics.setColor(java.awt.Color.WHITE);
graphics.fillRect(0, 0, width, height);
graphics.setColor(java.awt.Color.BLACK);
for (int x = 0; x < width; x++) {
for (int y = 0; y < height; y++) {
if (matrix.get(x, y) == true) {
graphics.fillRect(x, y, 1, 1);
}
}
}
try {
java.io.ByteArrayOutputStream bos=new java.io.ByteArrayOutputStream();
javax.imageio.ImageIO.write(image, filetype, bos );
bos.close();
Object imagebytes;
imagebytes=bos.toByteArray();
com.pega.pegarules.pub.util.Base64Util encoder=new com.pega.pegarules.pub.util.Base64Util();
String b64=encoder.encodeToString((byte[])imagebytes);
String b64string = b64.toString();
return b64string;
}
catch(Exception e) { throw new PRRuntimeException(e); }

