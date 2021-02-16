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

.. _contract-schema:

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
=============================================

Kung ang isang contract schema ay dapat na naka-embed o nakasulat sa isang file ay nakasalalay sa sitwasyon.

Embedding the schema into the smart contract module distributes the schema
together with the contract ensuring the correct schema is being used and also
allows anyone to use it directly. The downside is that the smart contract module
becomes bigger in size and therefore more expensive to deploy.
But unless the smart contract uses very complex types for the state and
parameters, the size of the schema is likely to be negligible compared to the
size of the smart contract itself.

Having the schema in a separate file allows you to have the schema without
paying for the extra bytes when deploying.
The downside is that you instead have to distribute the schema file through some
other channel and ensure that contract users are using the correct file with your
smart contract.

The schema format
=================

.. todo::

   Clarify whether we talk about *any* abstract schema that a user could implement,
   or a specific schema supplied by Concordium. Then only talk about one or the other,
   or at least clearly separate the discussion of those.

A schema can contain

- structure information for a smart contract module
- description of smart-contract state
- parameters for init and receive functions of a smart contract.

Each of these descriptions is referred to as a *schema type*. Schema types are always
optional to include in a schema.

Currently the supported schema types are inspired by what is commonly used in
the Rust programming language:

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


Here, ``SizeLength`` describes the number of bytes used to describe the length
of a variable length type, such as ``List``.

.. code-block:: rust

   enum SizeLength {
       One,
       Two,
       Four,
       Eight,
   }

For a reference on how a schema type is serialized into bytes, we refer the
reader to the `implementation in Rust`_.

.. _contract-schema-which-to-choose:

Embedding schemas on-chain
==========================

Schemas are embedded into smart contract modules using the `custom
section`_ feature of Wasm modules.
This allows Wasm modules to include a named section of bytes, which does not
affect the semantics of running the Wasm module.

All schemas are collected and added in one custom section named
``concordium-schema-v1``.
This collection is a list of pairs, containing the name of the contract encoded
in UTF-8 and the contract schema bytes.
