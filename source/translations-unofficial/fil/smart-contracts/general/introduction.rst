.. Should answer:
    - What is a smart contract
    - Why use a smart contract
    - What are the use cases
    - What are not the use cases

.. _introduction:

===================================
Introduksyon sa mga smart contracts
===================================

Ang smart contract ay pirase ng code na user-supplied na isinumite sa Concordium blockchain, 
ito ay ginagamit upang matukoy ang ugali na hindi direktang parte ng core na protocol. 
Ang mga smart contracts ay ineexecute ng mga node sa Concordium network ayon sa paunang natukoy na mga patakaran. 
Ang pagsasagawa nito ay ganap na malinaw, at lahat ng nodes ay dapat sumangayon sa kung ano ang kakalabasan ng eksekusyon
at magbabase lang sa kung ano ang impormasyon na bukas sa publiko. 

Ang smart contract ay maaaring makatanggap, maghawak at magpadala ng GTU, maaari din itong makapag obserba ng ibang aspeto ng chain, 
at ma-maintain ang kanyang sariling estado. Ang mga smart contract ay laging napapatupad bilang pagresponde sa mga **external** na mga aksyon,
halimbawa, ang account na nagpapadala ng mensahe. Sa pagsasagawa, ang mga smart contract ay madalas na magiging maliit na parte ng mas malaking sistema, 
na pinagsama sama ang functionality ng on at off chain. Ang halimbawa ng off-chain functionality ay pwedeng server na nakakapag invoke ng smart contract
na base sa data na galing sa tunay na mundo, tulad ng mga presyo ng mga stock, o impormasyon galing sa panahon.


Para saan ba ang mga smart contract?
====================================

Ang mga smart contract ay nakakabawas ng kinakailangan tiwala sa mga third-party, sa ibang mga kaso pwedeng tanggalin na ang pangangailangan ng pinagkakatiwalaang third-party, sa ibang mga kaso mababawasan ang kanilang kapabilidad at dahil dun mababawasan ang tiwala na kailangan para sa kanila.

Dahil ang mga smart contract ay napapatupad ng buo at maliwanag, sa pamamaraan na kahit sino na may access sa node ay maaaring magberipika, maaari din silang maging napaka mainam sa pangsigurado ng mga kasunduan sa gitna ng mga partido. 

.. _auction:

Halimbawa ng smart contract na pang subasta
-------------------------------------------

Ang isang pwedeng pag gamitan ng mga smart contract ay ang paggawa ng subastahan; Dito pinoprogram
ang smart contract para tumanggap ng ibat-ibang mga bid sa kahit na sino at mai-track ang pinaka 
mataas na taga pusta. 
Kapag tapos na ang subasta, ang smart contract ay magpapadala ng GTU sa tagabenta na galing sa may panalong pusta
at yung mga ibang pusta ay ibabalik. Dapat ipadala ng tagabenta ang item sa nanalo. 

Ang smart contract ay papalit sa pangunahing papel ng auctioneer. Ang kontrata ang tanging 
nag gogovern sa bidding part, at sa on-chain na pagpapamahagi ng GTUs. Ito ay mangangailangan din ng 
konting lohika para sa pagsauli ng taya kung sakaling hindi magampanan ng pinaka mataas na pumusta 
ang kanyang mga tunkulin. Ito ay nangangahulugan na ang kontrata ay kailangang magkaroon ng karampatang
suporta na pagkakapatunayan na ang tagabenta ay tunay na nagampanan ang kanyang tunkulin, o paraan para ang 
pinaka mataas na pumusta ay makapag hain ng reklamo kung sakali. Ang mga smart contract ay hindi nakakalutas
ng mga real-world na isyu ng kusa, at ang pinaka mainam na solusyon at nakadepende malamang sa mga specifics 
ng subusta.


Para saan *hindi na-aangkop* ang mga smart contract?
====================================================

Ang mga smart contract ay sobrang nakaka-excite na teklolohiya at ang mga tao ay 
nakakahanap pa ng mga bagong paraan kung papaano ito sasamantalahin. 
Kaso, may mga kaso din na ang mga smart contract ay hindi mainam na solusyon. 

Isa sa mga kalamangan ng mga smart contract ay ang tiwala sa pagpapatupad ng code, 
at para magampanan ito may malaking bilang ng mga node sa loob ng network ng blockchain 
ang kailangang magpatupad ng kaparehong code para masigurado ang kasunduan para sa resulta. 
Natural, ito ay magiging mas mahal, kumpara sa pagtakbo ng parehong code ng isang node na 
tumatakbo sa ibang cloud service.


Sa mga kaso na ang mga smart contract ay dumedepende sa mabibigat na mga kalkulasyon, 
maaaring posible na ilipat ang mga kalkulasyon sa labas ng smart contract at ipatupad lang
sa smart contract ang ibang mahalagang bahagi ng komputasyon, gamit ang mga teknik na cryptographic 
para masigurado ang ibang parte ay mapatupad ng tama. 

Sawakas, important na matandaan na ang smart contract ay hindi pribado at
**lahat** ng smart contract ay na may-access ay maaring ma-access din ng iba sa
Concordium network, ibig sabihin mahirap hawakan ang sensitibong data sa
smart contract. Sa iilang kaso posibleng gumamit ng cryptographic tools para
hindi direktang makikita ang data, ngunit naayon na ang smart contracts ang gumagawa sa
derived notions tulad ng encryptions at commitments, na natatago ang aktwal na data.


Siklo ng buhay ng smart contract
================================

Ang smart contract ay unang na-ideploy sa chain bilang parte ng :ref:`contract
module <contract-module>`. Pagkatapos nito ang smart contract ay maaring mag *initialized* para
makuha ang :ref:`smart contract instance <contract-instances>`. Sawakas ang smart
contract instance ay maaring paulit-ulit na ma-update ayon sa sariling lohika.
