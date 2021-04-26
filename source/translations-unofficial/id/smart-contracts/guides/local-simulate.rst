.. _local-simulate-id:

==========================================
Mensimulasikan fungsi kontrak secara lokal
==========================================

Panduan ini membahas tentang cara mensimulasikan pemanggilan beberapa init atau
fungsi penerimaan secara lokal dari modul kontrak pintar Wasm dalam konteks dan
status tertentu.
Simulasi ini berguna untuk memeriksa kontrak pintar dan hasilnya dalam
skenario tertentu.

.. seealso::

   Untuk panduan tentang pengujian unit otomatis, lihat :ref:`unit-test-contract-id`.

Persiapan
===========

Pastikan anda telah menginstal ``cargo-concordium``,jika belum silahkan ikuti
:ref:`setup-tools-id`.
Anda juga memerlukan modul smart contract di Wasm untuk mensimulasikan.

.. todo::

   Tulis sisanya, saat skema sudah siap.

Mensimulasikan Instansiasi
==========================

Untuk menyimulasikan pembuatan instance smart contract menggunakan
``cargo-concordium``, jalankan perintah berikut:

.. code-block:: console

   $cargo concordium run init --module contract.wasm \
                               --contract "my_contract" \
                               --context init-context.json \
                               --amount 123456.789 \
                               --parameter-bin parameter.bin \
                               --out-bin state.bin

``init-context.json`` (digunakan dengan parameter ``--context``) adalah file yang
berisi informasi konteks seperti status rantai saat ini, pengirim transaksi,
dan akun mana yang menjalankan fungsi ini.
Contoh dari konteks ini bisa jadi:

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

   Untuk referensi konteks, lihat :ref:`simulate-context`.


Mensimulasikan pembaruan
========================

Untuk mensimulasikan pembaruan ke sebuah instance smart contract menggunakan
``cargo-concordium``, jalankan:

.. code-block:: console

   $cargo concordium run update --module contract.wasm \
                                 --contract "my_contract" \
                                 --func "some_receive" \
                                 --context receive-context.json \
                                 --amount 123456.789 \
                                 --parameter-bin parameter.bin \
                                 --state-bin state-in.bin \
                                 --out-bin state-out.bin

``receive-context.json`` (digunakan dengan parameter ``--context``) adalah file yang
berisi informasi konteks seperti status rantai saat ini, pengirim transaksi,
dan akun mana yang menjalankan fungsi ini, dan akun atau
alamat yang mana yang mengirim pesan saat ini.
Contoh dari konteks ini bisa jadi:

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

   Untuk referensi konteks, lihat :ref:`simulate-context`.
