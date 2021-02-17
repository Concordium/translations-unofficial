.. _unit-test-contract-fil:

============================
Unit test a contract in Rust
============================

Ang gabay na eto ay mag papakita kung paano mag sulat ng unit tests para sa smart contract
sa Rust.
Para sa pag test ang smart contract Wasm modulo, tignan ang :ref:`local-simulate`.

This guide will show you how to write unit tests for a smart contract written in
Rust.
For testing a smart contract Wasm module, see :ref:`local-simulate`.

Ang smart contract sa Rust ay sinulat bilang library at makakapag unit test tayo dito
katulad ng isang library sa pagdagdag ng function na mayroong ``#[test]``attribute.

A smart contract in Rust is written as a library and we can unit test it like a
library by annotating functions with a ``#[test]`` attribute.

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

Running the test can be done using ``cargo``:

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
==================

Ang unit tests ay typical na sumusunod sa 3 parteng istraktura na kung saan ikaw:
ay nagsesetup ng kaunting estado, ang pagtakbo ng ibang yunit ng code,
gumawa ng mga pahayag tungkol sa estado at kalagayan ng code.

Unit tests typically follow a three-part structure in which you: set up some
state, run some unit of code, and make assertions about the state and output of
the code.

Kung ang contract functions ay nakasulat gamit ang ``#[init(..)]` o
``#[receive(..)]``,  magagawa nating itest ang function direkta sa unit test.

If the contract functions are written using ``#[init(..)]`` or
``#[receive(..)]``, we can test these functions directly in the unit test.

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

Testing stubs for the function arguments can be found in a submodule of
``concordium-std`` called ``test_infrastructure``.

.. seealso::

   Para sa iba pang impormasyon at halimbawa tignan ang pag gawa ng 
   dokyumento ng  concordium-std.
   For more information and examples see the crate documentation of
   concordium-std.

.. todo::

   Ipakita ang iba pang pag susulat ng unit test
   Show more of how to write the unit test

Pagtakbo ng tests sa Wasm
=====================

Compiling the tests to native machine code is sufficient for most cases, but it
is also possible to compile the tests to Wasm and run them using the exact
interpreter that is used by the nodes.
This makes the test environment closer to the run environment on-chain and could
in some cases catch more bugs.

Ang pag compile ng tests sa isang native machine code ay sapat na sa madadalas
na kaso, pero posible din icompile ang tests sa Wasm at patakbuhin sila gamit
ang eksakto interpreter na gunamit sa mga nodes.
Gagawin nito ang test na kapaligiran mas malapit sa ipinatatakbong kapaligiran sa on-chain
at sa ibang kaso mahuli ang mga bugs.


The development tool ``cargo-concordium`` includes a test runner for Wasm, which
uses the same Wasm-interpreter as the one shipped in the Concordium nodes.

Ang development tool na ``cargo-concordium`` ay may kasamang test runner para sa
Wasm, na kung saan gumagamit eto ng parehong Wasm-interpreter na katulad sa
pinadalang Concordium nodes.

.. seealso::

   For a guide of how to install ``cargo-concordium``, see :ref:`setup-tools`.
   Para sa gabay kung pano iinstall ang ``cargo-concordium``, tignan ang :ref:`setup-tools`.
   
Ang unit test ay kailangan lagyan ng ``#[concordium_test]`` imbis na
``#[test]``, at gumagamit ng ``#[concordium_cfg_test]`` imbis na ``#[cfg(test)]``:

The unit test have to be annotated with ``#[concordium_test]`` instead of
``#[test]``, and we use ``#[concordium_cfg_test]`` instead of ``#[cfg(test)]``:

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

The ``#[concordium_test]`` macro sets up our tests to be run in Wasm, when
``concordium-std`` is compiled with the ``wasm-test`` feature, and otherwise
falls back to behave just like ``#[test]``, meaning it is still possible to run
unit tests targeting native code using ``cargo test``.

Ganun din ang macro ``#[concordium_cfg_test]`` kasama ang ating module kapag bumuo
Ang "concordium-std" na may "wasm-test" kung hindi man ay kumikilos tulad ng "# [test]",
na nagpapahintulot sa atin na makontrol kung kailan isasama ang mga pagsubok sa pagbuo.

Similarly the macro ``#[concordium_cfg_test]`` includes our module when build
``concordium-std`` with ``wasm-test`` otherwise behaves like ``#[test]``,
allowing us to control when to include tests in the build.

Tests can now be build and run using:
Ang Tests ay mabubuo gamit ang:

.. code-block:: console

   $cargo concordium test
   
Ang command na eto ay nag cocompile sa tests para sa Wasm kasama ang ``wasm-test``
na pinagana para sa ``concordium-std``at gamit ang test runner mula sa  ``cargo-concordium``.

This command compiles the tests for Wasm with the ``wasm-test`` feature enabled
for ``concordium-std`` and uses the test runner from ``cargo-concordium``.

.. warning::

   Error messages from ``panic!``, and therefore also the different variations
   of ``assert!``, are *not* shown when compiling to Wasm.
   
   Ang mga error messages mula sa  ``panic!``, at ang iba pang variations ng
   ``assert!``,  ay *not* shown kapag nag cocompile sa Wasm.
   
   Sa halip gumamit ng ``fail!`` at ``claim!`` variants para sa mga pahayag
   kapag nag tetest, 

   Instead use ``fail!`` and the ``claim!`` variants to do assertions when
   testing, as these reports back the error messages to the test runner *before*
   failing the test.
   Both are part of ``concordium-std``.

.. todo::

   Use link concordium-std: docs.rs/concordium-std when crate is published.
