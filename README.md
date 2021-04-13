
# Unofficial Translations for Concordium Documentation

[![Contributor Covenant](https://img.shields.io/badge/Contributor%20Covenant-2.0-4baaaa.svg)](https://github.com/Concordium/.github/blob/main/.github/CODE_OF_CONDUCT.md)

This repository contains _**unofficial**_ translations for the [Official Concordium Documentation](https://developers.concordium.com/).

These are community contributed translations that have not been vetted and may not be completely accurate.

Please be careful when following any instructions in these docs, especially around keys, account addresses and transactions.

⚠️⚠️ If you are unsure, always compare to the [Official Concordium Documentation](https://developers.concordium.com/). ⚠️⚠️


### Contributing

Contributions are currently accepted via the [Testnet 4 Challenges](https://github.com/Concordium/Testnet4-Challenges) program.

Please go there to see the current contribution terms and rewards.


### Submission

Please read the [Official Concordium Documentation README](https://github.com/Concordium/concordium.github.io/blob/main/README.md) for the [Style Guide](https://github.com/Concordium/concordium.github.io/blob/main/README.md#style-guide) instructions.

Please keep the structure and ordering of document content as identical to the English source as possible.

Please create only one PR per language.


### File structure

Translations should go into the `source/translations-unofficial/<language code>` folder.

For the `<language code>` please use [IETF's BCP 47](https://www.w3.org/International/questions/qa-choosing-language-tags) "language subtag" standard.

For example:

- Chinese: `source/translations-unofficial/zh`
- Turkish: `source/translations-unofficial/tr`
- Korean: `source/translations-unofficial/ko`
- Russian: `source/translations-unofficial/ru`


Inside the `source/translations-unofficial/<language code>` folder, please duplicate the exact file structure & naming of the original `source/` file.

For example:

```
original:    source/smart-contracts/guides/compile-module.rst
translated:  source/translations-unofficial/zh/smart-contracts/guides/compile-module.rst
```
