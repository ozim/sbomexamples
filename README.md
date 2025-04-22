## Generowanie własnego
sbom-tool generate -b ./dotnet-test-application/bin/Debug/net9.0 -pn weather.ozimek.dev -pv 0.0.2 -ps ozimek.dev  
sbom2dot --input-file ./dotnet-test-application/bin/Debug/net9.0/_manifest/spdx_2.2/manifest.spdx.json -o manifest.dot  
dot -Tpng -o manifest.png manifest.dot   

cdxgen -o bom.json  
sbom2dot --input-file bom.json -o bom.dot  
dot -Tpng -o bom.png bom.dot   

## Tworzenie pliku dot i png z istniejacego z dużą ilością bibliotek
sbom2dot --input-file c2patool-v0.14.0-universal-apple-darwin-sbom.json -o c2patool.dot  
dot -Tpng -o c2patool.png c2patool.dot  

# sbom to doc
sbom2doc --input c2patool-v0.14.0-universal-apple-darwin-sbom.json -f markdown -o c2patolreadable.txt  
sbom2doc --input bom.json -f pdf -o bom.pdf  
sbom2doc --input manifest.spdx.json -f pdf -o manifestspdx.pdf  

# sbom audit

sbomaudit --input-file demo-click.json  

sbomaudit --input ./dotnet-test-application/bin/Debug/net9.0/_manifest/spdx_2.2/manifest.spdx.json  
sbomaudit --input bom.json  
sbomaudit --disable-license-check --input c2patool-v0.14.0-universal-apple-darwin-sbom.json  


# po prostu JQ
jq '.packages[].name' c2patool-v0.14.0-universal-apple-darwin-sbom.json  
jq '.packages[] | {name: .name, version: .versionInfo}' c2patool-v0.14.0-universal-apple-darwin-sbom.json  
jq '.packages[] | select(.name | contains("x509")) | {name: .name, version: .versionInfo}' c2patool-v0.14.0-universal-apple-darwin-sbom.json  
jq '.packages[] | select(.licenseConcluded | contains("MIT")) | {name: .name, version: .versionInfo, license: .licenseConcluded}' c2patool-v0.14.0-universal-apple-darwin-sbom.json
