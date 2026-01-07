
 class="revogado"><del>

# VERIFICAR QUAIS IDs FALTAM NA SEQUÊNCIA
(() => {
  // 1️⃣ Obtém o conteúdo do campo de texto
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

  // 2️⃣ Cria container temporário com o HTML do textarea
  const container = document.createElement("div");
  container.innerHTML = html;

  // 3️⃣ Extrai todos os IDs de artigos (artN ou artNletra)
  const todosIdsArt = Array.from(container.querySelectorAll("p[id^='art']"))
    .map(p => p.id)
    .filter(id => /^art\d+[a-z]?$/.test(id))
    .map(id => ({
      id,
      numero: parseInt(id.replace(/[^\d]/g, ""), 10)
    }))
    .sort((a, b) => a.numero - b.numero);

  if (todosIdsArt.length === 0) {
    console.warn("⚠️ Nenhum <p id='art...'> encontrado no texto.");
    return;
  }

  // 4️⃣ Verifica lacunas na sequência
  const faltantes = [];
  for (let i = 1; i < todosIdsArt.length; i++) {
    const esperado = todosIdsArt[i - 1].numero + 1;
    if (todosIdsArt[i].numero !== esperado) {
      for (let j = esperado; j < todosIdsArt[i].numero; j++) {
        faltantes.push(`art${j}`);
      }
    }
  }

  // 5️⃣ Relatório no console
  console.log("📘 ===== VERIFICAÇÃO DE SEQUÊNCIA DE ARTIGOS =====");
  console.log(`📄 Total de artigos encontrados: ${todosIdsArt.length}`);
  console.log("📚 IDs detectados:", todosIdsArt.map(x => x.id));

  if (faltantes.length) {
    console.warn(`⚠️ Faltam os seguintes artigos na sequência: ${faltantes.join(", ")}`);
  } else {
    console.log("✅ Nenhum artigo faltando na sequência — tudo ok!");
  }

  console.log("===============================================");
})();
