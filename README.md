# Circuitpython_Universal_Sockets
The UniversalSocket class to abstract native and ESP32 sockets to feed into something taking either.

Example using the HTTPServer library with ESP32SPI:
```py
from universal_socket import UniversalSocket
from adafruit_esp32spi import adafruit_esp32spi
import adafruit_esp32spi.adafruit_esp32spi_socket as socket
from adafruit_httpserver.server import HTTPServer

esp32_cs = DigitalInOut(board.ESP_CS)
esp32_ready = DigitalInOut(board.ESP_BUSY)
esp32_reset = DigitalInOut(board.ESP_RESET)

spi = busio.SPI(board.SCK, board.MOSI, board.MISO)
esp = adafruit_esp32spi.ESP_SPIcontrol(
    spi, esp32_cs, esp32_ready, esp32_reset
)

esp.connect_AP(
    os.getenv("CIRCUITPY_WIFI_SSID"),
    os.getenv("CIRCUITPY_WIFI_PASSWORD"),
)

socket.set_interface(esp)
usock = UniversalSocket(socket, iface=esp)
server = HTTPServer(usock)
IP_ADDRESS = "%d.%d.%d.%d" % tuple(esp.ip_address)

server.serve_forever(host=str(IP_ADDRESS), port=80, root_path="/www")
```
