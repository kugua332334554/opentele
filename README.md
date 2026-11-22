<!-- vim: syntax=Markdown -->

# opentele


A **Python Telegram API Library** for converting between **tdata** and **telethon** sessions, with built-in **official Telegram APIs**.

**FORK BY thedemons/opentele**

## random device generation

generate a random device fingerprint for any supported client. pass `unique_id` to get the same fingerprint every time, omit it to get a new device every call.

```python
from opentele.api import API

api = API.TelegramIOS.Generate(unique_id="my.session")
api = API.TelegramAndroid.Generate()
api = API.TelegramDesktop.Generate("windows", unique_id="my.session")
api = API.TelegramWeb_K.Generate(unique_id="my.session")
```

sample output = `device_model` / `system_version` / `app_version` / `lang_pack`

| client | data in | sample output |
|---|---|---|
| `API.TelegramIOS` | `unique_id="my.session"` | `iPhone 8` / `14.5.1` / `8.4` / `ios` |
| `API.TelegramAndroid` | `unique_id="my.session"` | `Huawei LND-AL30` / `SDK 24` / `8.4.1 (2522)` / `android` |
| `API.TelegramAndroidX` | `unique_id="my.session"` | `Huawei LND-AL30` / `SDK 24` / `8.4.1 (2522)` / `android` |
| `API.TelegramMacOS` | `unique_id="my.session"` | `MacBook Pro` / `macOS 10.14.5` / `8.4` / `macos` |
| `API.TelegramDesktop` | `system="windows"` (or `"macos"`, `"linux"`; omit for random), `unique_id="my.session"` | `N68-GS3 UCC` / `Windows 7` / `3.4.3 x64` / `tdesktop` |
| `API.TelegramWeb_Z` | `unique_id="my.session"` | `Mozilla/5.0 (Macintosh; ...) Safari/605.1.15` / `MacIntel` / `13.0.2 A` / `weba` |
| `API.TelegramWeb_K` | `unique_id="my.session"` | `Mozilla/5.0 (Macintosh; ...) Safari/605.1.15` / `MacIntel` / `2.3 K` / `webk` |
| `API.Webogram` | `unique_id="my.session"` | `Mozilla/5.0 (Macintosh; ...) Safari/605.1.15` / `MacIntel` / `0.7.0` / `""` |

## Dependencies

- [telethon](https://github.com/LonamiWebs/Telethon) - Widely used Telegram's API library for Python.
- [tgcrypto](https://github.com/pyrogram/tgcrypto) - AES-256-IGE encryption to works with `tdata`.
- [pyQt5](https://www.riverbankcomputing.com/software/pyqt/) - Used by Telegram Desktop to streams data from files.

## Installation
- Install from [PyPI](https://pypi.org/project/opentele-kugua/):
```pip title="pip"
pip install --upgrade opentele-kugua
```

## First Run
Load TDesktop from tdata folder and convert it to telethon, with a custom API:
```python
from opentele.td import TDesktop
from opentele.tl import TelegramClient
from opentele.api import API, CreateNewSession, UseCurrentSession
import asyncio

async def main():
    
    # Load TDesktop client from tdata folder
    tdataFolder = r"C:\Users\<username>\AppData\Roaming\Telegram Desktop\tdata"
    tdesk = TDesktop(tdataFolder)

    # Using official iOS API with randomly generated device info
    # print(api) to see more
    api = API.TelegramIOS.Generate()

    # Convert TDesktop session to telethon client
    # CreateNewSession flag will use the current existing session to
    # authorize the new client by `Login via QR code`.
    client = await tdesk.ToTelethon("newSession.session", CreateNewSession, api)

    # Although Telegram Desktop doesn't let you authorize other
    # sessions via QR Code (or it doesn't have that feature),
    # it is still available across all platforms (APIs).

    # Connect and print all logged in devices
    await client.connect()
    await client.PrintSessions()

asyncio.run(main())
```
