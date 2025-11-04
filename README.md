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

# linki, materiały źródłowe

https://spdx.dev/  
https://cyclonedx.org/  
https://www.ntia.gov/sites/default/files/publications/vex_one-page_summary_0.pdf  
https://www.cisa.gov/resources-tools/resources
https://pages.nist.gov/swid-tools/  
https://github.com/CycloneDX  
https://ecma-international.org/publications-and-standards/standards/ecma-424/  
https://www.iso.org/standard/65666.html  
https://csrc.nist.gov/projects/software-identification-swid/guidelines  
https://www.cisa.gov/sbom  
https://lfx.linuxfoundation.org/  
https://owasp.org/blog/2025/02/24/advisory-on-implementation-of-software-bill-of-materials-for-vulnerability-management.html
https://spdx.dev/use/spdx-tools/


[https://www.linkedin.com/pulse/generating-software-bill-materials-aka-sboms-scale-atlassian-seth-pbwfe](https://www.linkedin.com/pulse/generating-software-bill-materials-aka-sboms-scale-atlassian-seth-pbwfe/?trackingId=3lAf5jtPQqGLsRndfU0nMw%3D%3D)
https://www.cisa.gov/resources-tools/groups/enduring-security-framework-esf
https://github.com/oasis-tcs/csaf/tree/master
https://www.linkedin.com/pulse/generating-software-bill-materials-aka-sboms-scale-atlassian-seth-pbwfe
https://tools.spdx.org/app/compare/
https://cloud.google.com/software-supply-chain-security/docs/overview
https://cloud.google.com/assured-open-source-software/docs/overview
https://devblogs.microsoft.com/engineering-at-microsoft/generating-software-bills-of-materials-sboms-with-spdx-at-microsoft/
https://outshift.cisco.com/blog/top-10-supply-chain-attacks
https://github.com/DependencyTrack/dependency-track/issues/1746

https://docs.github.com/en/code-security/supply-chain-security/understanding-your-software-supply-chain/exporting-a-software-bill-of-materials-for-your-repository

https://docs.github.com/en/code-security/getting-started/github-security-features
https://outshift.cisco.com/blog/if-your-business-asks-what-sbom-is-it-is-failing

https://www.legitsecurity.com/blog/what-is-an-sbom-sbom-explained-in-5-minutes?utm_source=chatgpt.com

https://www.ntia.gov/other-publication/2021/ntia-software-component-transparency
https://www.practical-devsecops.com/recommended-sbom-consumption-practices/
https://www.cisa.gov/sites/default/files/2024-08/SECURING_THE_SOFTWARE_SUPPLY_CHAIN_RECOMMENDED_PRACTICES_FOR_SOFTWARE_BILL_OF_MATERIALS_CONSUMPTION-508.pdf
https://github.com/openvex/spec/releases
