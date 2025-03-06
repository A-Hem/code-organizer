# Smart Home Control Server
from mcp import FastMCP
import requests

app = FastMCP("smart-home")

@app.tool()
async def control_light(
    room: str,  # Bedroom, Kitchen, Living Room
    state: str   # on/off/dim
):
    """Control smart lights using Philips Hue API"""
    hue_bridge_ip = "192.168.1.100"
    response = requests.put(
        f"http://{hue_bridge_ip}/api/lights/{room}/state",
        json={"on": state == "on", "bri": 100 if state == "dim" else 254}
    )
    return {"status": "success" if response.ok else "failed"}

# Now Claude can: "Turn on kitchen lights to 50% brightness"