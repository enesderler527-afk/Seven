<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Seven Personel Sistemi</title>
    <style>
        body { font-family: 'Segoe UI', sans-serif; background: #121212; color: white; padding: 15px; }
        .seven-card { background: #1e1e1e; padding: 20px; border-radius: 15px; border: 1px solid #333; margin-bottom: 20px; }
        h2 { color: #00d4ff; text-align: center; }
        input, textarea { width: 100%; padding: 12px; margin: 8px 0; border-radius: 8px; border: none; background: #2a2a2a; color: white; }
        button { width: 100%; padding: 12px; border-radius: 8px; border: none; font-weight: bold; cursor: pointer; }
        .btn-save { background: #00d4ff; color: #000; margin-top: 10px; }
        .product-item { background: #252525; padding: 15px; border-radius: 10px; margin-top: 15px; border-left: 4px solid #00d4ff; }
        .btn-wa { background: #25D366; color: white; margin-top: 10px; }
        .btn-edit { background: #555; color: white; margin-top: 5px; font-size: 12px; width: auto; padding: 5px 15px; }
    </style>
</head>
<body>

<div class="seven-card">
    <h2>SEVEN</h2>
    <p style="text-align: center; font-size: 12px; color: #888;">Personel Takip & Satış</p>
    
    <input type="text" id="pAd" placeholder="Adınız Soyadınız">
    <input type="tel" id="pTel" placeholder="Telefon (Örn: 90530...)">
    <input type="password" id="pSifre" placeholder="Düzenleme Şifreniz">
    <textarea id="pUrun" placeholder="Ürün Tanıtımı..."></textarea>
    <button class="btn-save" onclick="ekle()">Kullanıcı Ekle ve Yayınla</button>
</div>

<div id="liste"></div>

<script>
    let veriler = JSON.parse(localStorage.getItem('seven_data')) || [];

    function ekle() {
        const ad = document.getElementById('pAd').value;
        const tel = document.getElementById('pTel').value;
        const sifre = document.getElementById('pSifre').value;
        const urun = document.getElementById('pUrun').value;

        if(!ad || !tel || !sifre) return alert("Lütfen tüm alanları doldurun!");

        const yeni = { id: Date.now(), ad, tel, sifre, urun };
        veriler.push(yeni);
        localStorage.setItem('seven_data', JSON.stringify(veriler));
        listele();
        alert("Sisteme eklendiniz!");
    }

    function listele() {
        const listeDiv = document.getElementById('liste');
        listeDiv.innerHTML = '<h3>Satıştaki Ürünler</h3>';
        veriler.forEach(item => {
            listeDiv.innerHTML += `
                <div class="product-item">
                    <strong>${item.ad}</strong><br>
                    <small>${item.urun}</small><br>
                    <button class="btn-wa" onclick="wa('${item.tel}', '${item.urun}')">WhatsApp'tan Fotoğraf İste</button>
                    <button class="btn-edit" onclick="duzenle('${item.id}')">Düzenle</button>
                </div>
            `;
        });
    }

    function wa(tel, urun) {
        const url = `https://wa.me/${tel}?text=${encodeURIComponent('Merhaba, Seven uygulamasındaki ' + urun + ' ürünü için fotoğraf alabilir miyim?')}`;
        window.open(url, '_blank');
    }

    function duzenle(id) {
        const sifre = prompt("Düzenlemek için şifrenizi girin:");
        const urun = veriler.find(u => u.id == id);
        if(urun && urun.sifre === sifre) {
            const yeniTanitim = prompt("Ürün tanıtımını güncelleyin:", urun.urun);
            if(yeniTanitim) {
                urun.urun = yeniTanitim;
                localStorage.setItem('seven_data', JSON.stringify(veriler));
                listele();
            }
        } else { alert("Hatalı şifre!"); }
    }
    listele();
</script>
</body>
</html>
