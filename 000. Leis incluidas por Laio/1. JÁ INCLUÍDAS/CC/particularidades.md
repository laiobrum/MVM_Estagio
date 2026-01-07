# Artigos 1.000 e 2.000 
Fiz à mão a correção do span titles

# Correção dos IDs 
Além do botão de correção dos IDs, executei este código no console:
(() => {
  // 1️⃣ Pega o conteúdo do campo de texto
  const textarea = document.querySelector("textarea[name='texto']");
  if (!textarea) {
    alert("❌ Campo <textarea name='texto'> não encontrado na página.");
    return;
  }

  const html = textarea.value.trim();
  if (!html) {
    alert("❌ O campo de texto está vazio.");
    return;
  }

  // 2️⃣ Cria container temporário para manipular o HTML
  const container = document.createElement("div");
  container.innerHTML = html;

  const corrigidos = [];

  // 3️⃣ Corrige IDs de artigos com ponto (Art. 1.000. → art1000)
  container.querySelectorAll("p").forEach(p => {
    const titles = p.querySelector("span.titles");
    if (!titles) return;

    const raw = (titles.textContent || "").trim();

    // Captura números com pontos e possível sufixo (ex: Art. 1.234-A)
    const match = raw.match(/Art\.?\s*(\d{1,3}(?:\.\d{3})*)(?:-([A-Za-z]))?/i);
    if (!match) return;

    const numero = match[1].replace(/\./g, ""); // remove pontos (1.234 → 1234)
    const letra = match[2] ? match[2].toLowerCase() : "";
    const novoId = `art${numero}${letra}`;

    if (p.id !== novoId) {
      corrigidos.push(`${p.id || "(sem id)"} → ${novoId}`);
      p.id = novoId;
    }
  });

  // 4️⃣ Verifica a sequência de IDs
  const todosIdsArt = Array.from(container.querySelectorAll("p[id^='art']"))
    .map(p => p.id)
    .filter(id => /^art\d+[a-z]?$/.test(id))
    .map(id => parseInt(id.replace(/[^\d]/g, ""), 10))
    .sort((a, b) => a - b);

  const faltantes = [];
  for (let i = 1; i < todosIdsArt.length; i++) {
    const esperado = todosIdsArt[i - 1] + 1;
    if (todosIdsArt[i] !== esperado) {
      for (let j = esperado; j < todosIdsArt[i]; j++) {
        faltantes.push(j);
      }
    }
  }

  // 5️⃣ Atualiza o campo com o HTML corrigido
  textarea.value = container.innerHTML;

  // 6️⃣ Relatório no console
  console.log("📘 ===== VERIFICAÇÃO E CORREÇÃO DE IDS =====");
  console.log(`✅ ${corrigidos.length} IDs corrigidos (formato artN).`);
  if (corrigidos.length) console.table(corrigidos.map(c => ({ "Correção": c })));
  console.log("📚 Todos os IDs artN encontrados:", todosIdsArt.map(n => `art${n}`));
  if (faltantes.length) {
    console.warn(`⚠️ Falta(m) os seguintes artigos na sequência: ${faltantes.map(n => `art${n}`).join(", ")}`);
  } else {
    console.log("✅ Nenhum artigo faltando na sequência — tudo ok!");
  }
  console.log("================================");
})();
