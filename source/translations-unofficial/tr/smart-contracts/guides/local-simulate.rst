.. _local-simulatetr:

=======================================
Sözleşme işlevlerinin yerel simülasyonu
=======================================

Bu kılavuz, bazı init uygulamalarının yerel olarak nasıl simüle edileceği veya
belirli bir içerikte veya durumda olan Wasm akıllı kontrat modülünün alış
fonksiyonu hakkındadır.
Bu simülasyon, akıllı bir sözleşmeyi incelemek ve belirli bir senaryonun
çıktısı için kullanışlıdır.

.. seealso::

   Otomatik birim testleriyle ilgili kılavuz için :ref:`unit-test-contract`.

Hazırlık
========

Kılavuza uymadıysanız, öncelikle `cargo-concordium`` un kurulu olduğundan emin olun. :ref:`setup-tools`. Simüle etmek için Wasm'da akıllı bir sözleşme modülüne de ihtiyacınız olacaktır.

.. todo::

   Şema maddeleri hazır olduğunda gerisini tamamlayın..

Örnekleme simülasyonu
=====================

``cargo-concordium`` kullanarak bir akıllı sözleşme örneklemesini simüle etmek için, aşağıdaki komutu çalıştırın:

.. code-block:: console

   $cargo concordium run init --module contract.wasm \
                               --contract "my_contract" \
                               --context init-context.json \
                               --amount 123456.789 \
                               --parameter-bin parameter.bin \
                               --out-bin state.bin

``init-context.json`` (``--context`` parametresiyle birlikte kullanılır) zincirin mevcut durumunu, işlemin gönderenini ve bu işlevi hangi hesabın çalıştırdığını içeren bir dosyadır.
Bu içeriğin bir örneği şu şekildedir:

.. code-block:: json

   {
       "metadata": {
           "slotTime": "2021-01-01T00:00:01Z"
       },
       "initOrigin": "3uxeCZwa3SxbksPWHwXWxCsaPucZdzNaXsRbkztqUUYRo1MnvF",
       "senderPolicies": [{
           "identityProvider": 1,
           "createdAt": "202012",
           "validTo": "202109"
       }],
   }

.. seealso::

   Bağlam referansı için bkz :ref:`simulate-context`.


Güncelleme simülasyonu
======================

``cargo-concordium`` kullanarak akıllı bir sözleşmeye güncelleme simülasyonu
uygulamak için, bunu çalıştırın:

.. code-block:: console

   $cargo concordium run update --module contract.wasm \
                                 --contract "my_contract" \
                                 --func "some_receive" \
                                 --context receive-context.json \
                                 --amount 123456.789 \
                                 --parameter-bin parameter.bin \
                                 --state-bin state-in.bin \
                                 --out-bin state-out.bin


``receive-context.json`` (``--context`` parametresiyle birlikte kullanılır) zincirin mevcut durumunu, işlemin gönderenini, bu işlevi hangi hesabın çalıştırdığını ve mevcut mesajı hangi hesap veya adresin gönderdiği gibi bilgileri içeren bir dosyadır.
Bu içeriğin bir örneği şu şekildedir:

.. code-block:: json

   {
       "metadata": {
           "slotTime": "2021-01-01T00:00:01Z"
       },
       "invoker": "3uxeCZwa3SxbksPWHwXWxCsaPucZdzNaXsRbkztqUUYRo1MnvF",
       "selfAddress": {"index": 0, "subindex": 0},
       "selfBalance": "0",
       "sender": {
           "type": "account",
           "address": "3uxeCZwa3SxbksPWHwXWxCsaPucZdzNaXsRbkztqUUYRo1MnvF"
       },
       "senderPolicies": [{
           "identityProvider": 1,
           "createdAt": "202012",
           "validTo": "202109"
       }],
       "owner": "3uxeCZwa3SxbksPWHwXWxCsaPucZdzNaXsRbkztqUUYRo1MnvF"
   }

.. seealso::

   İçerik referensı için bkz :ref:`simulate-context`.
