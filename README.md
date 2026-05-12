<h1>EstigVirtualRoom-Demo</h1>
<h3>Ambiente Virtual 3D em WebGL — ESTIG / IPBeja</h3>

Sala virtual interactiva e multiplayer desenvolvida em Unity 6, focada na simulação da presença física na Escola Superior de Tecnologia e Gestão (ESTIG) do Instituto Politécnico de Beja. O projecto permite que utilizadores explorem o edifício em tempo real, comuniquem entre si e interajam com elementos do espaço.

---

## Sobre o Projecto

Este ambiente virtual foi desenvolvido com o objectivo de criar uma representação digital do campus da ESTIG. Os utilizadores acedem através de autenticação institucional, escolhem um avatar personalizado e são colocados num espaço 3D partilhado onde podem movimentar-se livremente, comunicar e interagir com o ambiente e com outros utilizadores presentes em simultâneo.

---

## Tecnologias

| Tecnologia | Utilização |
|---|---|
| **Unity 6** | Motor de jogo e build WebGL |
| **Firebase Authentication** | Autenticação por email/password via REST API |
| **Cloud Firestore** | Base de dados NoSQL para perfis e avatares |
| **Photon PUN2** | Multiplayer em tempo real |
| **GLTFast** | Carregamento de avatares GLB em runtime |
| **Mixamo** | Personagens NPC e animações |
| **Blender** | Modelação e preparação de assets 3D |
| **Nginx** | Servidor web para build WebGL com compressão Gzip |

---

## Funcionalidades

**Autenticação**
- Registo e login com email institucional (`@ipbeja.pt` / `@stu.ipbeja.pt`)
- Verificação de email obrigatória antes do acesso
- Sessão persistente com "Lembrar-me" via refresh token
- Recuperação de password por email

**Avatar**
- Selecção de avatar a partir de uma lista com pré-visualização em tempo real
- Avatar guardado no perfil do utilizador no Firestore
- Carregamento de ficheiros GLB locais via GLTFast (sem dependências externas)

**Multiplayer**
- Sala partilhada com até 10 utilizadores em simultâneo via Photon PUN2
- Sincronização de posições, movimentos e animações em tempo real
- Lista de jogadores online visível durante a sessão

**Chat**
- Mensagens públicas para toda a sala
- Mensagens privadas entre utilizadores específicos
- Notificação de novas mensagens quando o chat está fechado

**Exploração e Interactividade**
- Movimento em terceira pessoa com câmara orbital
- NPCs com animações
- Interacção com NPCs via tecla E com diálogos contextuais
- Painéis informativos activados por proximidade

---

## Arquitectura

```
Firebase Auth (REST API)
    ↓ autenticação
Cloud Firestore
    ↓ perfil + avatar ID
Unity WebGL (GLTFast)
    ↓ carrega GLB local
Photon PUN2
    ↓ sincroniza multiplayer
```

A comunicação com o Firebase é feita exclusivamente via REST API, sem SDKs nativos e garantindo compatibilidade total com WebGL.

---

## Notas de Desenvolvimento

<h4>Migração Ready Player Me → Sistema Local GLB</h4>
O projecto foi originalmente desenvolvido com integração à plataforma Ready Player Me (RPM), que disponibilizava um SDK para Unity e uma API REST que permitia a criação e carregamento dinâmico de avatares 3D altamente personalizados. Cada utilizador criava o seu avatar único através do editor online do RPM, escolhendo características faciais, roupa, cabelo e acessórios. O resultado era guardado como um URL único por utilizador no Firestore. Em runtime, o SDK do RPM descarregava o avatar directamente a partir desse URL e instanciava-o na cena Unity.
Em Dezembro de 2025, a plataforma Ready Player Me foi adquirida pela Netflix e os seus serviços públicos foram encerrados a 31 de Janeiro de 2026, tornando todos os URLs de avatares inválidos e o SDK completamente inoperacional.

A migração afectou múltiplas camadas do projecto, tais como a Base de dados, o Carregamento em runtime, o Armazenamento de avatares, a Interface de selecção, o sistema Multiplayer entre outros.

---

## Requisitos para Deploy

- Servidor web 
- Build WebGL do Unity com compressão **Gzip** activa
- Configuração para servir `.unityweb` com `Content-Encoding: gzip`
- Pasta `StreamingAssets/Avatares/` com os ficheiros `.glb` dos avatares

---
## Demonstração

<div align="center">
  <table>
    <tr>
      <td align="center">
        <img width="400" alt="Login" src="https://github.com/user-attachments/assets/10134e49-7a7c-4ca2-8e68-bbbe39b1a29f" />
        <br/>
        <em>Interface de login</em>
      </td>
      <td align="center">
      <img width="400" alt="Login" src="https://github.com/user-attachments/assets/2a3073d5-17f7-444a-854e-93252bad1028" /> 
        <br/>
        <em>Selecção de avatar com pré-visualização em tempo real</em>
      </td>
    </tr>
    <tr>
      <td align="center">
        <img width="400" alt="Jogo" src="https://github.com/user-attachments/assets/b23b9660-36de-4df1-b3d5-23f51d4467db" />  
        <br/>
        <em>Sala H2O com NPCs e ambiente interactivo</em>
      </td>
      <td align="center">
        <img width="400" alt="Chat" src="https://github.com/user-attachments/assets/a9d199c4-6339-4e4d-82f0-47996df4e1aa" />
        <br/>
        <em>Ambiente multiplayer com avatares sincronizados</em>
      </td>
    </tr>
     <tr>
      <td align="center">
        <img width="400" alt="Jogo" src="https://github.com/user-attachments/assets/39fc3392-1e62-475f-b7b7-ed3e0b64edc9" />
        <br/>
        <em>Chat de texto com mensagens públicas</em>
      </td>
      <td align="center">
        <img width="400" alt="Chat" src="https://github.com/user-attachments/assets/928ff04d-1c39-42da-a8e7-83b15ec998c1" />
        <br/>
        <em>Placard de Interação </em>
      </td>
    </tr>
         <tr>
      <td align="center">
        <img width="400" alt="Jogo" src="https://github.com/user-attachments/assets/07f82951-5f45-4cd0-9d00-0fbc95106b81" />  
        <br/>
        <em>Sistema de Notificações </em>
      </td>
      <td align="center">
        <img width="400" alt="Chat" src="https://github.com/user-attachments/assets/e9c9fdcc-e2f5-47f1-8e78-0ce84450f48a" /> 
        <br/>
        <em>Interação com NPC </em>
      </td>
    </tr>
  </table>
</div>

## Créditos

Projecto desenvolvido por **Pedro Dionísio** para o 
 **Instituto Politécnico de Beja (IPBeja) / ESTIG**.

## Acesso ao Código Fonte

O código fonte é propriedade do IPBeja e está disponível apenas para acesso institucional.
