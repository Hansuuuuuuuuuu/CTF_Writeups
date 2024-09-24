That's my WiFi
> https://shorturl.at/N6LHy

#### Points: 75

We are given just a shortened link. Visiting the said link leads to a page that gives the real hint:

> The owner of the network titled Vango24 has two cars parked in front of their home. A black nissan versa and a blue ?

Based on the title of the challenge, `Vango24` is a WiFi SSID. Luckily, there's a service that maps WiFi networks called WiGLE, which might have the said network.

After logging on to WiGLE, I went to the search page and entered `Vango24` on the `SSID / Network Name` field. This results in 2 hits, both around the same area. Using the map on the right pane, I zoomed in and placed the street view marker on the upper dot, which ended up being the correct one as I immediately saw two vehicles, one black and the other blue. I then used Google Lens to figure out the make of the car - Saturn Ion, which was the flag.


`FLAG{Saturn Ion}`