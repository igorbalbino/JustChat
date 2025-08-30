# P2PChat – MVP de Chat Descentralizado E2E

## 🌟 Visão Geral

P2PChat é um aplicativo de chat **descentralizado, seguro e minimalista**, com mensagens **E2E (end-to-end)**, identidade mínima e interface simples.  
O app é projetado para ser **cross-platform (iOS, Android, eventualmente desktop)**, mantendo a privacidade e o desempenho da rede.

### Objetivos principais

- **Identidade mínima:** ID curto derivado de chave pública + username opcional
- **Mensagens E2E:** Double Ratchet, CBOR/MessagePack, replay protection, TOFU automático
- **Rede P2P:** DHT Kademlia, NAT traversal, conexão direta P2P, fallback relay cego-encriptado
- **Armazenamento local:** SQLCipher / Keychain / Keystore, índices de chats, TTL automático
- **UI minimalista:** Onboarding, Meus chats, busca de usuários
- **Segurança e performance:** operações críticas em Rust, RN apenas para interface e fluxo

---

## 🛠 Funcionalidades

1. **Identidade do usuário**
   - Par de chaves Ed25519/X25519 gerado na instalação
   - ID curto (10–12 dígitos) derivado da chave pública
   - Username opcional, repetível
   - Novo dispositivo → novo ID e chaves

2. **Mensagens**
   - Formato CBOR/MessagePack (`msg_id`, `ratchet_header`, `ciphertext`, `signature`, `timestamp`, `ttl`)
   - Double Ratchet por conversa
   - Replay protection
   - TOFU / detecção de downgrade / MITM automática

3. **Rede P2P**
   - DHT Kademlia assinada (TTL curto, replicação, anti-entropy)
   - Lookup por ID ou prefixo de username
   - NAT traversal (hole punching via QUIC / WebRTC)
   - Fallback relay cego-encriptado (TTL + rate-limit)

4. **Armazenamento local**
   - Mensagens e índices de chats armazenados apenas no dispositivo
   - SQLCipher / Keychain / Keystore para segurança
   - Limpeza automática por TTL

5. **UI**
   - Onboarding (username → ID curto)
   - Meus chats
   - Busca de usuários
   - Visualização de mensagens com status (pendente / entregue / falha)

6. **Testes**
   - Segurança, replicação DHT, NAT traversal, relay, performance
   - Multi-node simulation via Docker Compose

---

## 💻 Tecnologias e Frameworks

| Camada | Linguagem / Framework | Observações |
|--------|--------------------|-------------|
| UI / UX | React Native + TypeScript | Multiplataforma, integra com Rust via JSI/FFI |
| Criptografia | Rust (`ed25519-dalek`, `x25519-dalek`, `libsignal-rs`) | Chaves, Double Ratchet, assinatura/verificação, replay protection |
| Rede P2P | libp2p-rs + libp2p JS | DHT Kademlia, NAT traversal, relay cego-encriptado |
| Serialização | CBOR / MessagePack | Estrutura leve e rápida para mensagens |
| Armazenamento | SQLCipher + Keychain / Keystore | Chats, mensagens, replay cache |
| Testes | Rust unit tests + Jest + Docker Compose | Testes de segurança, NAT, replicação, relay, performance |

---

## 🛠 Roadmap de Desenvolvimento

1. **Core Criptografia e Identidade**
   - Gerar chave Ed25519/X25519 e ID curto
   - Armazenar chaves no Keychain/Keystore
   - Linguagem: Rust + React Native FFI/JSI

2. **Estrutura de Mensagens e Double Ratchet**
   - Formato CBOR/MessagePack
   - Implementar Double Ratchet
   - Replay protection e detecção de downgrade
   - Linguagem: Rust

3. **Armazenamento Local**
   - SQLCipher DB com tabelas: `chats`, `messages`, `peers`
   - TTL e índice de chats
   - Linguagem: Rust bindings + RN

4. **DHT Kademlia e Descoberta de Usuários**
   - Lookup por ID/username
   - Replicação, TTL curto, anti-entropy
   - Linguagem: Rust libp2p + JS libp2p para RN

5. **Conexão P2P Direta e NAT Traversal**
   - Hole punching QUIC / WebRTC
   - Linguagem: Rust + JS libp2p

6. **Fallback Relay Cego-encriptado**
   - Armazenar mensagens cifradas temporariamente
   - TTL + rate-limit
   - Linguagem: Rust (libp2p circuit)

7. **UI Minimalista**
   - Onboarding, Meus Chats, Busca de usuários, Chat individual
   - Linguagem: React Native + TypeScript

8. **Testes e Validação**
   - Unit tests, multi-node, stress tests, segurança
   - Linguagem: Rust + Docker Compose + Jest (UI)

---

## ⚡ Boas Práticas

- Async/threads para operações pesadas → não bloquear UI
- Modularização: Rust → criptografia e ratchet, RN → UI e fluxo
- Chaves privadas **nunca** no JS/DB exposto
- Logs mínimos → nunca exibir dados sensíveis
- Testes contínuos e simulação multi-node

---

## 📁 Estrutura Inicial de Pastas

