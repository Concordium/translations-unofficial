.. Should answer:
..
.. - Why should I use a schema?
.. - What is a schema?
.. - Where to use a schema?
.. - How is a schema embedded?
.. - Should I embed or write to file?
..

.. _`custom section`: https://webassembly.github.io/spec/core/appendix/custom.html
.. _`implementation in Rust`: https://github.com/Concordium/concordium-contracts-common/blob/main/src/schema.rs

.. _contract-schema-fil:

======================
Smart contract schemas
======================

Ang smart contract schema ay isang paglalarawan kung paano kumatawan sa mga bytes sa isang mas structured representation. Maaari itong magamit ng mga panlabas na mga tools kapag ipinapakita ang estado ng isang smart contract instance at para sa pagtukoy ng mga parameters gamit ang structured representation, katulad ng JSON.

.. seealso::

   For instructions on how to build the schema for a smart contract module in
   Rust, see :ref:`build-schema`.

Bakit gagamitin ang contract schema
===================================

Ang data sa blockchain, tulad ng estado ng isang instance at mga parameter na naipasa sa init at makatanggap ng mga functions, ay naka-serialize bilang sequence ng mga bytes. Ang serialization ay na-optimize para sa kahusayan, sa halip na madaling mabasa ng tao.

.. todo::

   Isaalang-alang ang muling pagsusulat ng subseksyon na ito dahil maaari itong maging medyo mahirap maunawaan; sa partikular, posibleng sabihin lamang na para sa kaginhawaan, ang gumagamit maaaring ipasa ang hindi naka-serialized na data sa isang function hangga't nagbibigay din sila ng isang schema na binabaybay kung paano ma-(de)serialize ang data.

Karaniwan ang mga byte na ito ay may istraktura at ang istrakturang ito ay kilala ng smart contract bilang bahagi ng contract function, ngunit sa labas ng mga function na ito ay maaaring maging mahirap na magkaroon ng kahulugan ang byte. Lalo na ito ang kaso kung kailan inspeksyon ng isang kumplikadong estado ng isang contract instance o kapag pumasa sa kumplikadong mga parameter sa isang smart contract function. Sa huling kaso, dapat ang mga byte alinman ay ma-serialize mula sa nakabalangkas na data o manu-manong nakasulat.

Ang solusyon para sa pag-iwas sa manu-manong pag-parse ng mga byte ay upang makuha ang impormasyong ito sa isang *smart contract schema*, na naglalarawan kung paano gumawa ng istraktura mula sa bytes, at maaaring magamit ng mga panlabas na tool.

.. note::

   Ang ``concordium-client`` tool ay pwedeng gumamit ng schema para sa
   :ref:`serialize JSON parameters<init-passing-parameter-json>`
   at i-deserialize ang stado ng contract instances sa JSON.

Ang schema ay maaaring naka-embed sa isang smart contract module na na-deploy sa chain, o nakasulat sa isang file at ipinasa sa off-chain.

Dapat mo bang i-embed o sumulat sa isang file?
==============================================

Kung ang isang contract schema ay dapat na naka-embed o nakasulat sa isang file ay nakasalalay sa sitwasyon.

Ang pag-embed sa iskema sa smart contract module ay namamahagi ng schema kasama ang contract na tinitiyak na ang tamang schema ay ginagamit at pinapayagan din ang sinuman na gamitin ito nang direkta. Ang downside ay ang smart contract module ay nagiging mas malaki at samakatuwid ay mas mahal na i-deploy. Ngunit maliban kung ang smart contract ay gumagamit ng mga kumplikadong uri para sa estado at mga parameter, ang laki ng schema ay malamang na bale-wala kumpara sa laki ng smart contract mismo.

Ang pagkakaroon ng schema sa isang hiwalay na file ay nagbibigay-daan sa iyo upang magkaroon ng schema nang walang pagbabayad para sa labis na byte kapag nagde-deploy.
Ang downside ay sa halip kailangan mong ipamahagi ang schema file sa ilan iba pang mga channel at tiyakin na ang mga gumagamit ng contract ay gumagamit ng tamang file sa iyong smart contract.

Ang schema format
=================

.. todo::

   Linawin kung pinag-uusapan natin ay tungkol sa *kahit anong* na maaaring ipatupad ng isang gumagamit,
   o isang spesipikong schema na ibinibigay ng Concordium. Pagkatapos ay pag-uusapan lamang ang tungkol sa isa o sa iba pa,
   o kahit papaano malinaw na paghiwalayin ang talakayan ng mga iyon.

Ang schema ay naglalaman

- istraktura ng impormasyon para sa isang smart contract module
- kahulugan ng smart-contract state
- parameters para sa init at makatanggap ng functions ng smart contract.

Ang bawat isa sa mga paglalarawan na ito ay tinukoy bilang *schema type*. Ang mga Schema types palagi opsyonal na isama sa isang schema.

Sa kasalukuyan ang mga sinusuportahang uri na schema ay inspirasyon ng kung ano ang karaniwang ginagamit sa Rust programming language:

.. code-block:: rust

   enum Type {
       Unit,
       Bool,
       U8,
       U16,
       U32,
       U64,
       I8,
       I16,
       I32,
       I64,
       Amount,
       AccountAddress,
       ContractAddress,
       Timestamp,
       Duration,
       Pair(Type, Type),
       List(SizeLength, Type),
       Set(SizeLength, Type),
       Map(SizeLength, Type, Type),
       Array(u32, Type),
       Struct(Fields),
       Enum(List (String, Fields)),
   }

   enum Fields {
       Named(List (String, Type)),
       Unnamed(List Type),
       Empty,
   }


Dito, ang ``SizeLength`` naglalarawan ng bilang ng mga byte na ginamit upang ilarawan ang haba
ng isang variable lenght type, tulad ng ``List``.

.. code-block:: rust

   enum SizeLength {
       One,
       Two,
       Four,
       Eight,
   }

Para sa isang sanggunian kung paano naka-serialize ang isang uri ng schema sa mga byte, nire-refer namin ang mga mambabasa sa `implementation in Rust`_.

.. _contract-schema-which-to-choose-fil:

Ang pang-embed ng mga schemas on-chain
======================================

Ang mga scheme ay naka-embed sa mga smart contract module gamit ang `custom
section`_ featuret ng Wasm modules.
Pinapayagan nito ang Wasm modules na isama ang mga named section of bytes, na di naman nakakaapekto sa
semantics ng pagpapatakbo sa Wasm module.

Ang lahat ng schemas ay nakakolekta at dinadagdag sa isang custom section na pinangalanang
``concordium-schema-v1``.
Ang koleksyon na ito ay listahan ng mga pares, naglalaman ng mga pangalan ng mga contracte na encoded sa
UTF-8 at ang contract schema bytes.
