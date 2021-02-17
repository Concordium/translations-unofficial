.. _unit-test-contract-fil:

============================
Unit test a contract in Rust
============================

Ang gabay na eto ay mag papakita kung paano mag sulat ng unit tests para sa smart contract
sa Rust.
Para sa pag test ang smart contract Wasm modulo, tignan ang :ref:`local-simulate`.

Ang smart contract sa Rust ay sinulat bilang library at makakapag unit test tayo dito
katulad ng isang library sa pagdagdag ng function na mayroong ``#[test]``attribute.

.. code-block:: rust

    // contract code
    ...

    #[cfg(test)]
    mod test {

        #[test]
        fn some_test() { ... }

        #[test]
        fn another_test() { ... }
    }

Pagtakbo ng test ay magagawa gamit ang ``cargo``:

.. code-block:: console

   $cargo test



By default, this command compiles the contract and tests to machine code for
your local target (most likely ``x86_64``), and runs them.
This kind of testing can be useful in initial development and for testing
functional correctness.
For comprehensive testing, it is important to involve the target platform, i.e.,
`Wasm32`.
There are a number of subtle differences between platforms, which can change the
behaviour of a contract.
One difference is regarding the size of pointers, where `Wasm32` uses four bytes
as opposed to eight, which is common for most platforms.





Pagsusulat unit tests
=====================

Ang unit tests ay typical na sumusunod sa 3 parteng istraktura na kung saan ikaw:
ay nagsesetup ng kaunting estado, ang pagtakbo ng ibang yunit ng code,
gumawa ng mga pahayag tungkol sa estado at kalagayan ng code.

Kung ang contract functions ay nakasulat gamit ang ``#[init(..)]` o
``#[receive(..)]``,  magagawa nating itest ang function direkta sa unit test.

.. code-block:: rust

   use concordium_std::*;

   #[init(contract = "my_contract", payable, enable_logger)]
   fn contract_init(
      ctx: &impl HasInitContext<()>,
      amount: Amount,
      logger: &mut impl HasLogger,
   ) -> InitResult<State> { ... }

   #[receive(contract = "my_contract", name = "my_receive", payable, enable_logger)]
   fn contract_receive<A: HasActions>(
      ctx: &impl HasReceiveContext<()>,
      amount: Amount,
      logger: &mut impl HasLogger,
      state: &mut State,
   ) -> ReceiveResult<A> { ... }

Pag tetest ng stubs para sa function arguments ay makikita sa submodule na
``concordium-std`` tawag ay ``test_infrastructure``.

.. seealso::

   Para sa iba pang impormasyon at halimbawa tignan ang pag gawa ng 
   dokyumento ng  concordium-std.

.. todo::

   Ipakita ang iba pang pag susulat ng unit test

Pagtakbo ng tests sa Wasm
=========================

Ang pag compile ng tests sa isang native machine code ay sapat na sa madadalas
na kaso, pero posible din icompile ang tests sa Wasm at patakbuhin sila gamit
ang eksakto interpreter na gunamit sa mga nodes.
Gagawin nito ang test na kapaligiran mas malapit sa ipinatatakbong kapaligiran sa on-chain
at sa ibang kaso mahuli ang mga bugs.

Ang development tool na ``cargo-concordium`` ay may kasamang test runner para sa
Wasm, na kung saan gumagamit eto ng parehong Wasm-interpreter na katulad sa
pinadalang Concordium nodes.

.. seealso::

   Para sa gabay kung pano iinstall ang ``cargo-concordium``, tignan ang :ref:`setup-tools`.
   
Ang unit test ay kailangan lagyan ng ``#[concordium_test]`` imbis na
``#[test]``, at gumagamit ng ``#[concordium_cfg_test]`` imbis na ``#[cfg(test)]``:

.. code-block:: rust

   // contract code
   ...

   #[concordium_cfg_test]
   mod test {

       #[concordium_test]
       fn some_test() { ... }

       #[concordium_test]
       fn another_test() { ... }
   }
   
Ang   ``#[concordium_test]`` macro ay nag tatalaga ng ating test na tumakbo
sa Wasm, kung ang ``concordium-std`` ay na compile gamit ang ``wasm-test`` 
na feature, kung hindi man bumabalik eto upang kumilos tulad ng ``#[test]``,
nangangahulugang posible pa ring tumakbo ang unit tests papunta sa native code
gamit ang ``cargo test``.

Ganun din ang macro ``#[concordium_cfg_test]`` kasama ang ating module kapag bumuo
Ang "concordium-std" na may "wasm-test" kung hindi man ay kumikilos tulad ng "# [test]",
na nagpapahintulot sa atin na makontrol kung kailan isasama ang mga pagsubok sa pagbuo.

Ang Tests ay mabubuo gamit ang:

.. code-block:: console

   $cargo concordium test
   
Ang command na eto ay nag cocompile sa tests para sa Wasm kasama ang ``wasm-test``
na pinagana para sa ``concordium-std``at gamit ang test runner mula sa  ``cargo-concordium``.

.. warning::
  
   Ang mga error messages mula sa  ``panic!``, at ang iba pang variations ng
   ``assert!``,  ay *not* shown kapag nag cocompile sa Wasm.
   
   Sa halip gumamit ng ``fail!`` at ``claim!`` variants para sa mga pahayag
   kapag nag tetest,  ang mga report na to ay sumasalo sa mga maling mensahe
   patungo sa test runner  *before* pumalya ang test.
   Parehas silang parte ng  ``concordium-std``.

.. todo::

   Use link concordium-std: docs.rs/concordium-std when crate is published.
   Gamitin ang link na concordium-std: docs.rs/concordium-std kung ang crate ay natala.
