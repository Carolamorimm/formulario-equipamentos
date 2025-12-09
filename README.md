# Formulário de Equipamentos

Um formulário web responsivo e moderno para cadastro de equipamentos de TI com integração para envio de dados.

## 🎯 Características

- ✅ Design responsivo e profissional
- ✅ Validação de campos em tempo real
- ✅ Suporte para upload de fotos
- ✅ Integração com serviços de email
- ✅ Feedback visual (sucesso/erro)
- ✅ Interface intuitiva e fácil de usar

## 📋 Campos do Formulário

1. **Portador** - Nome da pessoa responsável pelo equipamento
2. **Equipamento** - Tipo de equipamento (MacBook, Dell, Lenovo, etc.)
3. **Modelo** - Versão/modelo específico
4. **Serial Number** - Número de série do equipamento
5. **HD** - Capacidade de armazenamento
6. **Processador** - Tipo de processador
7. **Memória** - Quantidade de RAM
8. **Marca** - Fabricante do equipamento
9. **Seu e-mail** - Email de contato do portador
10. **Foto como evidência** - Imagem do equipamento (opcional)

## 🚀 Como Configurar e Usar

### Passo 1: Configurar o Formspree (Recomendado)

1. Acesse [formspree.io](https://formspree.io)
2. Crie uma conta gratuita
3. Crie um novo formulário
4. Copie o ID do formulário (formato: `xyzpqwer`)
5. No arquivo `index.html`, procure pela linha:
   ```javascript
   const response = await fetch('https://formspree.io/f/xyzpqwer', {
   ```
6. Substitua `xyzpqwer` pelo seu ID de formulário
7. Salve o arquivo

### Passo 2: Fazer Upload no GitHub

1. Acesse seu repositório: https://github.com/Carolamorimm/formulario-equipamentos
2. Clique em "Add file" → "Upload files"
3. Faça upload dos arquivos:
   - `index.html` (o formulário)
   - `README.md` (documentação)
4. Commit os arquivos

### Passo 3: Ativar GitHub Pages

1. Vá em **Settings** do repositório
2. Clique em **Pages** (no menu lateral)
3. Em "Source", selecione "main branch"
4. Salve as alterações
5. Seu formulário estará disponível em: `https://carolamorimm.github.io/formulario-equipamentos/`

### Passo 4: Usar o Formulário

1. Acesse a URL do GitHub Pages
2. Preencha todos os campos obrigatórios
3. Opcionalmente, anexe uma foto do equipamento
4. Clique em "Enviar Formulário"
5. Você receberá uma confirmação de sucesso
6. Os dados serão enviados para seu email (via Formspree)

## 📱 Responsividade

O formulário funciona perfeitamente em:
- Desktop
- Tablet
- Mobile

## 🎨 Personalização

### Cores
Edite as cores no CSS (seção `<style>`):
- Gradiente principal: `#667eea` e `#764ba2`
- Cor de sucesso: `#d4edda`
- Cor de erro: `#f8d7da`

### Fontes
Mude a fonte no CSS:
```css
body {
    font-family: 'Sua Fonte Aqui', sans-serif;
}
```

### Campos
Para adicionar ou remover campos, modifique a seção HTML do formulário.

## 🔒 Segurança

- Validação de email no frontend
- Validação de arquivo (apenas imagens)
- Proteção contra CSRF com Formspree
- Dados enviados via HTTPS

## 📊 Dados Coletados

Os dados são coletados em tempo real e podem ser:
- Salvos em email (Formspree)
- Armazenados em Google Sheets (com configuração adicional)
- Enviados para seu banco de dados

## 🐛 Troubleshooting

### Formulário não envia
- Verifique se o ID do Formspree está correto
- Confira a conexão com a internet
- Abra o console (F12) para ver mensagens de erro

### Foto não é enviada
- Certifique-se que o arquivo é uma imagem
- Verifique o tamanho do arquivo (máximo 5MB recomendado)
- Tente com outro formato (JPG, PNG)

### Mensagens de erro
- Verifique o console do navegador (F12 → Console)
- Verifique se a URL de envio está correta
- Teste a conexão com a internet

## 📝 Notas

- O campo de foto é opcional
- Todos os outros campos são obrigatórios
- Os dados são enviados em tempo real
- Não há limite de envios

## 🔗 Links Úteis

- [GitHub Pages](https://pages.github.com/)
- [Formspree](https://formspree.io/)
- [Google Sheets](https://sheets.google.com/)

## 📄 Licença

Este projeto é de código aberto e pode ser usado livremente.

---

**Desenvolvido com ❤️ para melhorar o controle de equipamentos**
