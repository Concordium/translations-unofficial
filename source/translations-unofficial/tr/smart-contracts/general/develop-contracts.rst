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

=========================================
Rust'ta akıllı sözleşmeler geliştirmek
=========================================

Concordium blockchain'de akıllı sözleşmeler Wasm modülleri olarak yüklenir,
ancak Wasm öncelikle derleme amacı ile tasarlanmıştır ve elle yazmak uygun değildir.
Bunun yerine akıllı sözleşmelerimizi Wasm'a derlemek için iyi bir desteğe
sahip olan Rust_ programlama dilinde yazabiliriz.


Akıllı sözleşmelerin sadece Rust ile yazılmasına gerek yoktur. Sunduğumuz ilk SDK'dır.
Manuel olarak yazılmış Wasm veya C, C ++, AssemblyScript_ ve diğer diller ile
derlenen Wasm, zincirde eşit derecede geçerlidir :ref:`Wasm limitations we impose <wasm-limitations>`.


.. seealso::

   Aşağıda açıklanan işlevler hakkında daha fazla bilgi için, Rust'taki Concordium
   blok zincirinde akıllı sözleşmeler yazmak için concordium_std_ API' dokumanına bakabilirsiniz.

.. seealso::

   Akıllı sözleşme modülleri hakkında daha fazla bilgi için bkz :ref:`contract-moduletr`.

   Rust'ta bir kütüphane olarak akıllı bir sözleşme modülü geliştirilir ve daha sonra Wasm'da derlenir.
   Doğru dışa aktarımları elde etmek için Manifest dosyasındaki `crate-type` özelliği
   şu şekilde ayarlanmalıdır: ``["cdylib", "rlib"]``


.. code-block:: text

   ...
   [lib]
   crate-type = ["cdylib", "rlib"]
   ...

``concordium_std`` kullanarak akıllı bir sözleşme geliştirmek
===================================================================

Akıllı sözleşme modülleri geliştirmek ve bilgisayar fonksiyonlarını
çağırmak için daha Rust benzeri bir deneyim sağlayan ``concordium_std``
sandığını kullanmanız önerilir.

Baslangic (init) ve alma fonksiyonları; sırasıyla ``#[init(...)]``
and ``#[receive(...)]`` basit Rust fonksiyonları gibi etkinleştirilir.

Altta bir sayaç uygulayan akıllı bir sözleşme örneğini göreceksiniz:

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

Burada dikkat edilmesi gereken birkaç nokta vardır:

.. todo::

   - Write up the requirements in an easier to read way (e.g., split up paragraphs into sub-bullets).
   - These requirements should be part of a specification that is written up somewhere,
     i.e., not just as part of this example.

- Fonksiyonların türü:

  * Bir init fonksiyonu, ``MyState``, ``Serialize`` özelliğini uygulayan; ``&impl HasInitContext -> InitResult<MyState>``  türünde bir fonksiyon olmalıdır.
  * Alma işlevi bir ``A: HasActions``, bir ``&impl HasReceiveContext``, ve bir ``&mut MyState`` almalı ve bir ``ReceiveResult<A>`` döndürmelidir.

- ``#[init(contract = "counter")]`` notu, uygulandığı işlevi ``counter`` adlı
  sözleşmenin başlatma işlevi olarak işaretler.
  Somut olarak, bu, bu makronun perde arkasında gerekli imza ve ``init_counter``
  adıyla dışa aktarılan bir fonksiyon oluşturduğu anlamına gelir.


- ``#[receive(contract = "counter", name = "increment")]`` serileştirmeyi kaldırır
  ve doğrudan işlenecek duruma getirir. Perde arkasında bu ek açıklama aynı zamanda
  gerekli imzaya sahip olan ``counter.increment`` adıyla dışa aktarılan bir işlev
  oluşturur ve serileştirmeyi kaldıran tum yapılarını gerekli tür olan ``State`` yapar.

.. note::

   Serileştirmenin maliyetsiz olmadığını ve bazı durumlarda kullanıcının bilgisayar
   fonksiyonlarının kullanımı üzerinde daha ayrıntılı kontrol isteyebileceğini
   unutmayın. Bu tür kullanım durumları için ek açıklamalar, daha az ek yük yaratan
   ancak kullanıcının daha fazla çalişma yapmasını gerektiren bir ``low_level``
   seçeneğini destekler.

.. todo::

   - Describe low-level
   - Introduce the concept of host functions before using them in the note above


Serileştirilebilir durum ve parametreler
-----------------------------------------------

.. todo:: Clarify what it means that the state is exposed similarly to ``File``;
   preferably, without referring to ``File``.

Zincir üzerinde, bir örneğin durumu bir bayt dizisi olarak temsil edilir ve
Rust standart kütüphanesinin ``File`` arayüzü ile benzer bir arayüzde gösterilir.

Bu, (de-)serialization (serileştirme) işlevlerini içeren ``Serialize``
özelliği kullanılarak yapılabilir.

``concordium_std`` kütüphanesi, Rust standart kitaplığındaki çoğu tür için
bu özelliği ve uygulamaları içermektedir. Ayrıca, kullanıcı tanımlı yapılar
(structs) ve sıralamalar (enums) türetmek için makrolar içerir.


.. code-block:: rust

   use concordium_std::*;

   #[derive(Serialize)]
   struct MyState {
       ...
   }

Aynı durum, başlatma (init) parametreleri ve alma fonksiyonlari için de gereklidir.

.. note::

   Açıkçası, baytları sadece parametre tipimize göre serileştirmemiz gerekir,
   ancak birim testlerini yazarken türleri serileştirebilmek uygun olacaktır.

.. _working-with-parameters:

Parametrelerle çalışmak
------------------------

Başlatma (init) ve alma fonksiyonlarinin parametreleri, örnek durumu gibi bayt
dizileri olarak temsil edilir. Bayt dizileri doğrudan kullanılabildiği gibi,
yapılandırılmış verilerin içinde serileştirilmesi kaldırılmış veriye de dönüştürülebilir.


Bir parametrenin serisini kaldırmanın en basit yolu, `Get`_ özelliğinin içindeki
`get()`_ fonksiyonudur.

Örnek olarak, Aşağıdaki sözleşmede vurgulanan satırda ``ReceiveParameter``
parametresinin serileştirilmeyi kaldırıldığı görebilirsiniz:

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

Yukarıdaki alma işlevi, ihtiyaç duyulmadığında bile ``value`` serisini
kaldırdığı için verimsizdir, Orn, ``should_add`` ``false`` olduğunda.

Daha fazla kontrol ve verimlilik elde etmek için, `Read`_ özelliğini
kullanarak parametrenin serileşmesini kaldırabiliriz:

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

``value`` yalnızca ``should_add`` ``true`` ise serileştirmenin kaldırıldığına
dikkat edin.
Bu örnekte verimlilikteki kazanç minimum olsa da, daha karmaşık örnekler
için önemli bir etkisi olabilir.


``cargo-concordium`` ile akıllı bir sözleşme modülü oluşturmak
===============================================================

Rust derleyicisi, ``wasm32-unknown-unknown`` hedefini kullanarak Wasm'a derleme
için oldukça iyi bir desteğe sahiptir.
Bununla birlikte, ``--release``  ile derlenirken bile, ortaya çıkan yapı,
zincirdeki akıllı sözleşmeler için kullanışlı olmayan özel bölümlerde büyük hata
ayıklama bilgileri bölümleri içerir.

Yapıyı optimize etmek ve şema yerleştirme gibi yeni özelliklere izin vermek için,
akıllı sözleşmeler oluşturmak için ``cargo-concordium`` kullanmanızı öneririz.

.. seealso::

  ``cargo-concordium`` kullanarak nasıl inşa edileceğine ilişkin talimatlar için
  :ref:`compile-module`’e bakabilirsiniz.



Akıllı sözleşmeleri test etme
===============================

Birim testleri
---------------------

Sözleşme çağrılarını simüle edilmesi
-------------------------------------

En iyi pratikler (Uygulamalar)
==================================

Panik yapmayın
---------------

.. todo::

   Use trap instead.

Kara delikler oluşturmaktan kaçının
----------------------------------------

Kendisine gönderilen GTU miktarını kullanmak için akıllı bir sözleşme gerekli
değildir ve varsayılan olarak akıllı bir sözleşme, birinin bir GTU göndermesi
durumunda bir örneğin bakiyesini boşaltmak için herhangi bir davranış tanımlamaz.
Bu durumda GTU'lar sonsuza kadar * kaybolur * ve onları kurtarmanın her hangi
yolu olmazdı.

Bu nedenle, GTU ile ilgili olmayan akıllı sözleşmeler için, gönderilen GTU
miktarının sıfır olmasını sağlamak ve olmayan tüm çağrıları reddetmek iyi bir
uygulamadır.

Yoğun hesaplamaları zincir dışına taşıyın
---------------------------------------------


.. _Rust: https://www.rust-lang.org/
.. _Cargo: https://doc.rust-lang.org/cargo/
.. _AssemblyScript: https://github.com/AssemblyScript
.. _get(): https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html#tymethod.get
.. _Get: https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html
.. _Read: https://docs.rs/concordium-std/latest/concordium_std/trait.Read.html
.. _concordium_std: https://docs.rs/concordium-std/latest/concordium_std/
