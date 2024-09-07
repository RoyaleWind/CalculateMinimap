## Get Minimap Size Based on Safezone for NUI Web
```lua
function CalculateMinimap()
    local safezoneSize = GetSafeZoneSize()
    local aspectRatio = GetAspectRatio(false)

    if aspectRatio > 2 then aspectRatio = 16 / 9 end

    local screenWidth, screenHeight = GetActiveScreenResolution()
    local xScale = 1.0 / screenWidth
    local yScale = 1.0 / screenHeight

    local minimap = {
        width = xScale * (screenWidth / (4 * aspectRatio)),
        height = yScale * (screenHeight / 5.674),
        leftX = xScale * (screenWidth * (1.0 / 20.0 * ((math.abs(safezoneSize - 1.0)) * 10))),
        bottomY = 1.0 - yScale * (screenHeight * (1.0 / 20.0 * ((math.abs(safezoneSize - 1.0)) * 10)))
    }
    
    if aspectRatio > 2 then
        minimap.leftX = minimap.leftX + minimap.width * 0.845
        minimap.width = minimap.width * 0.76
    elseif aspectRatio > 1.8 then
        minimap.leftX = minimap.leftX + minimap.width * 0.2225
        minimap.width = minimap.width * 0.995
    end
    
    minimap.topY = minimap.bottomY - minimap.height

    return {
        width = minimap.width * screenWidth,
        height = minimap.height * screenHeight,
        left = minimap.leftX * 100,
        top = minimap.topY * 100
    }
end
```

# Example TSX 
```tsx
style={{
        left: minimap.left + "%",
        top: minimap.top + "%",
        width: minimap.width + "px",
        height: minimap.height + "px",
      }}
```
