import cv2
import streamlit as st
from streamlit_webrtc import VideoTransformerBase, webrtc_streamer
from pyzbar.pyzbar import decode

class BarcodeTransformer(VideoTransformerBase):
    def transform(self, frame):
        # Convertir el frame a un arreglo de NumPy en formato BGR
        img = frame.to_ndarray(format="bgr24")
        # Decodificar los códigos de barras encontrados en la imagen
        barcodes = decode(img)
        for barcode in barcodes:
            (x, y, w, h) = barcode.rect
            # Dibujar un rectángulo alrededor del código detectado
            cv2.rectangle(img, (x, y), (x + w, y + h), (0, 255, 0), 2)
            # Extraer la información del código
            barcode_data = barcode.data.decode("utf-8")
            barcode_type = barcode.type
            text = "{} ({})".format(barcode_data, barcode_type)
            # Escribir el texto sobre la imagen
            cv2.putText(img, text, (x, y - 10), cv2.FONT_HERSHEY_SIMPLEX,
                        0.5, (0, 255, 0), 2)
        return img

def main():
    st.header("Lector de Códigos de Barras con Streamlit")
    st.write("Activa la cámara para iniciar el escaneo de códigos de barras.")
    webrtc_streamer(key="barcode-scanner", video_transformer_factory=BarcodeTransformer)

if __name__ == "__main__":
    main()
