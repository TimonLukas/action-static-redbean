# Create static server executable - powered by redbean 🦞

This GitHub action creates a single-file server that:
* runs anywhere (see [jart/cosmopolitan](https://github.com/jart/cosmopolitan))
* serves static files (with optional fallback)
* is extensible through an extensive [scripting interface](https://redbean.dev/#lua)

## Usage
```yaml
- uses: TimonLukas/action-static-redbean@v1
  with:
    # specifies version to be fetched from redbean.dev
    version: "latest"
    # Location of the finished executable
    executable-name: "redbean.com"
    # Location of static files to include
    input: "dist"
    # Fallback file (for history API-based routing), `false` to disable
    fallback: "index.html"
    # On launch, open the users preferred browser pointing to this path, `false` to disable
    launch: "/"
    

# Example: static server for `./build/*` without fallback using redbean version x.y.z
- uses: TimonLukas/action-static-redbean@v1
  with:
    version: "x.y.z"
    input: "build"
    fallback: false
- run: mv ./redbean.com $FOO
```

## Extending behavior
Generally, anything beyond serving files statically can be approached using the LUA api. The fallback option itself is actually implemented as:

```lua
function OnHttpRequest()
    if not RoutePath() then
        ServeAsset('$FALLBACK')
    end
end
```

To add any behavior, add `.init.lua` with your own content into your `input` directory.
Depending on your project it might be worth to look into frameworks for redbean like [`fullmoon`](https://github.com/pkulchenko/fullmoon).
