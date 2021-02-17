.. Should answer:
    - Why write a smart contract using rust?
    - What are the pieces needed to write a smart contract in rust?
        - State
            - Serialized
            - Schema
        - Init
        - Receive
    - What sort of testing is possible
    - Best practices?
        - Ensure 0 amount
        - Don't panic
        - Avoid heavy calculations

.. _writing-smart-contracts:

======================================
Pag-develop ng smart contracts sa Rust
======================================

Sa concordium blockchain smart contract ay naka-deploy bilang Wasm modules, ngunit
pangunahing naka-disenyo ang Wasm bilang compilation target at hindi ito madali para
isulat kamay.
Sa halip maaring magsulat ng smart contracts sa Rust programming language, kung
saan may maayos na suporta para sa pag-compile sa Wasm.

Ang smart contracts ay hindi kailangan isulat sa Rust.
Ito lamang ang unang SDK na mabibigay namin.
Ang mano-manong pagsulat sa Wasm, o compiled na Wasm sa C, C++, AssemblyScript_, at
iba pa, ay parehas na pantay na wasto sa chain, hangga't ito ay sumusunod sa :ref:`Wasm
limitations we impose <wasm-limitations>`.

.. seealso::

   Para sa karagdagang impormasyon sa functions na nakalarawan sa baba, tignan ang concordium_std_
   API para sa pagsulat ng smart contracts sa Concordium blockchain sa Rust.

.. seealso::

   Tignan ang :ref:`contract-module` para sa karagdagang impormasyon tungkol sa smart contract modules.

Ang smart contract module ay gawa sa Rust bilang library crate, tapos
na-compile sa Wasm.
Para makuha ang tamang exports, ang `crate-type` attribute ay kailangan naka-set sa
``["cdylib", "rlib"]`` sa loob ng manifest file:

.. code-block:: text

   ...
   [lib]
   crate-type = ["cdylib", "rlib"]
   ...

Pagsulat sa smart contract gamit ang ``concordium_std``
=======================================================

Ito ay inirerekomenda na gamitin ang ``concordium_std`` crate, na nagbibigay
dagdag karanasan sa Rust sa pag-develop ng smart contract modules at pagtawag
sa host functions.

Ang crate ay nagpapagana sa pagsulat sa init at receive functions bilang simpleng Rust
functions na anotado sa ``#[init(...)]`` at ``#[receive(...)]``.

Ito ang mga halimbawa ng smart contract na nag-iimplement ng counter:

.. code-block:: rust

   use concordium_std::*;

   type State = u32;

   #[init(contract = "counter")]
   fn counter_init(
       _ctx: &impl HasInitContext,
   ) -> InitResult<State> {
       let state = 0;
       Ok(state)
   }

   #[receive(contract = "counter", name = "increment")]
   fn contract_receive<A: HasActions>(
       ctx: &impl HasReceiveContext,
       state: &mut State,
   ) -> ReceiveResult<A> {
       ensure!(ctx.sender().matches_account(&ctx.owner()); // Only the owner can increment
       *state += 1;
       Ok(A::accept())
   }

Merong mga bagay na dapat bigyan pansin:

.. todo::

   - Isulat ang requirements sa mas madaling paraang basahin (e.g., ipaghiwalay ang talaga sa sub-bullets).
   - Itong mga kinakailangan ay dapat maging parte ng detaly na nakasulat sa kung saan,
     i.e., hindi lang ito parte ng halimbawa.

- Mga uri ng functions:

  * Ang init function ay dapat uri ng ``&impl HasInitContext -> InitResult<MyState>``
    kung saan ``MyState`` ay uri na nagpapatupad ang ``Serialize`` trait.
  * Ang receive function ay kailangan kumuha ng ``A: HasActions`` na uri ng parameter,
    ang ``&impl HasReceiveContext`` at ang ``&mut MyState`` parameter, at ibalik
    ang ``ReceiveResult<A>``.

- Ang anotasyon ``#[init(contract = "counter")]`` ay nagmamarka ng function na
  inilapat sa init function ng contract na ``counter``.
  Ang ibig sabihin nito ay sa likod ng eksena ito ay bumubuo ng macro na-exported
  function sa kailangan na signatura na ``init_counter``.

- ``#[receive(contract = "counter", name = "increment")]`` ay nag-deserialize at
  mga gamit na estado na direktang minamanipula.
  Sa likod ng eksena itong anotasyon ay nagbibigay ng exported function na may pangalan
  ``counter.increment`` na may kailangang signaturo, at naggagawa sa lahat ng
  boilerplate ng deserializing ng estado sa kinakailangang uri ``State``.

.. note::

   Tandaan na ang deserialization ay may gastos, at sa ibang kaso ang
   user ay gusto ng mas malawak na kontrol sa paggamit ng host functions.
   Sa gantong kaso ang anotasyon ay nagsusuporta sa ``low_level`` na pagpipilian, na
   may mas kaunting overhead, ngunit marami ang kinakailangan galing sa user.

.. todo::

   - Tukuyin ang low-level
   - Ipakilala ang konsepto ng host functions bago gamitin ito sa nakasulat sa taas


Serializable na estado at parameters
------------------------------------

.. todo:: Ilinaw ang ibig sabihin ng ang estado ay nakalabas katulad sa ``File``;
   mas pabor na hindi tinutukoy ang ``File``.

On-chain, ang estado ng instance ay nirerepresenta biang byte array at nakalantad
sa parehong interface bilang ``File`` interface ng Rust standard library.

Ito ay maaring magawa gamit ang ``Serialize`` na trait na naglalaman ng (de-)serialization
functions.

Ang ``concordium_std`` crate ay kasama sa gantong trait at implementasyon para sa
madalas ng uri ng Rust standard library.
Ito rin ay naglalaman ng macros para sa pag-dervie ng trait para sa user-defined structs at
enums.

.. code-block:: rust

   use concordium_std::*;

   #[derive(Serialize)]
   struct MyState {
       ...
   }

Parehas na kailangan ang parameters para sa init at receive functions.

.. note::

   Ipinaghihigpit na kailangan lang na mag deserialize ng bytes sa paramater type,
   ngunit mas madali na mag-serialize ng types kapag nagsusulat ng unit tests.

.. _working-with-parameters:

Paggawa sa parameters
---------------------

Ang parameters sa init at receive functions ay parang sa instance
state, na nirerepresenta bilang byte arrays.
Habang ang byte array ay nagagamit direkta, pwede rin ito ma-deserialized sa
structured data.

Ang madaling paraan para mag-deserialize ng parameter ay sa pamamagitan ng `get()`_ function ng
`Get`_ trait.

Bilang halimbawa, tignan ang sumusunod na contract kung saan ang parameter
``ReceiveParameter`` ay deserialized sa naka-highlight na linya:

.. code-block:: rust
   :emphasize-lines: 24

   use concordium_std::*;

   type State = u32;

   #[derive(Serialize)]
   struct ReceiveParameter{
       should_add: bool,
       value: u32,
   }

   #[init(contract = "parameter_example")]
   fn init(
       _ctx: &impl HasInitContext,
   ) -> InitResult<State> {
       let initial_state = 0;
       Ok(initial_state)
   }

   #[receive(contract = "parameter_example", name = "receive")]
   fn receive<A: HasActions>(
       ctx: &impl HasReceiveContext,
       state: &mut State,
   ) -> ReceiveResult<A> {
       let parameter: ReceiveParameter = ctx.parameter_cursor().get()?;
       if parameter.should_add {
           *state += parameter.value;
       }
       Ok(A::accept())
   }

Ang receive function sa taas ay hindi epektibo na nag-deserialize pa ng
``value`` kahit hindi na kailangan, i.e. kapag ang ``should_add`` ay ``false``.

Para makakuha ng mas malawak na control, at sa gantong kaso, mas mabilis, maari nating i-deserialize ang
parameter gamit ang `Read`_ trait:

.. code-block:: rust
   :emphasize-lines: 7, 10

   #[receive(contract = "parameter_example", name = "receive_optimized")]
   fn receive_optimized<A: HasActions>(
       ctx: &impl HasReceiveContext,
       state: &mut State,
   ) -> ReceiveResult<A> {
       let mut cursor = ctx.parameter_cursor();
       let should_add: bool = cursor.read_u8()? != 0;
       if should_add {
           // Only decode the value if it is needed.
           let value: u32 = cursor.read_u32()?;
           *state += value;
       }
       Ok(A::accept())
   }

Pansin na ang ``value`` ay deserialized lamang kung ang ``should_add`` ay
``true``.
Habang ang pagdagdag ng efficiency ay minimal sa gantong halimbawa, maaring magkaroon
ng substantial impact para sa maraming mahirap na halimbawa.


Pagbuo ng smart contract sa module gamit ang ``cargo-concordium``
=================================================================

Ang Rust compiler ay may maayos na suporta sa pag-compile sa Wasm gamit ang
``wasm32-unknown-unknown`` target.
Sapagkat, kahit nag-cocompile sa ``--release`` ang resulta ng pagbuo ay naglalaman ng
malalaking bahagi ng debug information sa custom sections, na hindi napapakinabangan sa
smart contracts on-chain.

Para mag-optimize ng build at payagan ang bagong features tulad ng embedding schemas,
nirerekomenda na gamitin ang ``cargo-concordium`` para magbuo ng smart contracts.

.. seealso::

   Para sa tagubilin kung paano magbuo gamit ang ``cargo-concordium`` tignan
   :ref:`compile-module`.


Pagsubok sa smart contracts
===========================

Unit test sa stubs
------------------

Gayahin ang tawag ng contract
-----------------------------

Mainam na gawi
==============

Wag mag-panik
-------------

.. todo::

   Gumamit ng bitag nalang.

Iwasan ang paggawa ng black holes
---------------------------------

Ang smart contract ay hindi kailangan gamitin ang amount ng GTU na pinadala nito, at
bilang default ang smart contract ay hindi nagtutukoy ng kahit anong ugali sa pag-ubos ng balanse
ng isang instance, sa kaso na may nagpadala ng GTU.
Itong GTU ay habang-buhay na *lost*, at wala paraan para masalba ito.

Kung gayon magandang ensayo ang smart contracts na hindi nakatungo sa GTU,
para masigurado na ang bingiay na amount ng GTU ay wala at hindi tanggapin ang kahit
anong panawagan na hindi.

Lumipat ng mabigat na kalkulasyon sa off-chain
----------------------------------------------


.. _Rust: https://www.rust-lang.org/
.. _Cargo: https://doc.rust-lang.org/cargo/
.. _AssemblyScript: https://github.com/AssemblyScript
.. _get(): https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html#tymethod.get
.. _Get: https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html
.. _Read: https://docs.rs/concordium-std/latest/concordium_std/trait.Read.html
.. _concordium_std: https://docs.rs/concordium-std/latest/concordium_std/
