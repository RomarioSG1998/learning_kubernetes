# 🚀 Meu Guia Definitivo e Prático de Kubernetes

Este documento é o resumo completo de tudo que eu aprendi, digitei e implementei "sujando as mãos" do absoluto zero no Kubernetes.

## 🧠 Conceitos Fundamentais Aprendidos
Hoje eu vi na prática como as gigantes da tecnologia rodam aplicações usando os 5 pilares do Kubernetes.

1. **Pods:** O "átomo" do K8s. É a cápsula/bolha abstrata que roda 1 ou mais containers (Docker) lá dentro. Pods são efêmeros (sofrerão amnésia e morrerão).
2. **Deployments:** O "Patrão". É o gerente impiedoso que assina um contrato para manter X réplicas de um Pod online 24h por dia. Se um morrer (Self-Healing), o Deployment invoca outro instantaneamente no lugar.
3. **Services:** O "Recepcionista". Como os IPs dos Pods mudam toda hora quando morrem, o Service fornece um Endereço IP fixo, permanente e atua como um Balanceador de Carga (Load Balancer) distribuindo o tráfego de rede para os clones disponíveis.
4. **ConfigMaps:** A "Prancheta do Chefe". O local seguro e independente para salvar configurações (Chaves/Valores, Tema, Nomes) injetando-as como Variáveis de Ambiente no Pod, sem precisar sujar a imagem do Docker original.
5. **PersistentVolumeClaims (PVC):** O "HD Externo". Um pedido formal de disco rígido pra rede. O Kubernetes disponibiliza o disco e nós o plugamos (VolumeMounts) na barriga do nosso container, curando a "amnésia" dele. Se o container morrer, os dados guardados nessa pasta sobrevivem.

---

## 🛠️ A Minha "Folha de Cola" de Comandos (Cheatsheet)

**Comandos de Observação (O jeito de perguntar as coisas ao mestre):**
- `kubectl get nodes` -> Lista as máquinas do meu cluster.
- `kubectl get pods` -> Lista os meus containers rodando e mostra se subiram com saúde (Running).
- `kubectl get services` -> Lista as minhas portas de entrada e Load Balancers.
- `kubectl get cm` -> Lista os ConfigMaps (Pranchetas de Variáveis).
- `kubectl get pvc` -> Lista as minhas encomendas de HD (Volumes).

**Comandos de Ação:**
- `kubectl apply -f <arquivo.yaml>` -> Entrega a planta da arquitetura pro mestre criar ou atualizar toda a Matrix automaticamente.
- `kubectl delete -f <arquivo.yaml>` -> Forma limpa de apagar toda uma arquitetura de uma vez baseada no arquivo.
- `kubectl delete pod <nome-do-pod>` -> Comando "hacker" para assassinar um pod específico e testar o gerente recriando ele na mesma hora.

**Comandos de Debug (Quando dá Erro):**
- `kubectl describe pod <nome-do-pod>` -> Puxa a "ficha médica" completa. Lê os Eventos no final da ficha informando exatamente porquê o Pod tá falhando (Erro de Imagem, Erro de Disco, Falta de Recurso).
- `kubectl exec <nome-do-pod> -- <comando>` -> Executa um comando Linux por dentro da barriga do servidor online.
  * _Exemplo:_ `kubectl exec meu-pod -- env` (Para listar variáveis de ambiente injetadas).
  * _Exemplo:_ `kubectl exec meu-pod -- ls /` (Para listar as pastas).
- `kubectl port-forward pod/<nome-do-pod> 8080:80` -> Fura um túnel entre a porta da bolha isolada pra minha porta local do PC.

---

## 💣 Os 3 Maiores "Tropeços" do YAML (O que eu NUNCA mais vou esquecer)

1. **Maiúsculas vs Minúsculas (Case-Sensitive):** O Kubernetes e a linguagem YAML não perdoam erros de capitalização. `Kind: Deployment` quebra o código. O certo é `kind: Deployment`.
2. **A "Regra do Dois Pontos":** Em YAML, todo `:` obrigatoriamente deve ser seguido de um espaço em branco se for receber um valor depois. (`app:web` está errado, o certo é `app: web`).
3. **O Desastre da Indentação:** Tudo no K8s usa 2 ou 4 espaços rígidos para criar alinhamentos e hierarquias. Se a palavra `spec` estiver alinhada abaixo de `labels`, o Kubernetes surta dizendo que você não declarou a estrutura. Eles têm que estar perfeitamente alinhados pelo nível pai.

---

## 🏆 A Arquitetura Final que eu construí
Nossa estrutura continha:
- `deployment.yaml`: Criava 3 réplicas do Nginx e conectava todas as partes soltas (Secrets, ConfigMaps e Discos).
- `service.yaml`: Expunha os Nginx na porta externa 30080 pro meu navegador.
- `configmap.yaml`: Injetava os títulos do meu site dinamicamente.
- `pvc.yaml`: Separou 1 Gigabyte de armazenamento persistente.

*Feito do Absoluto Zero! Ninguém digitou pra mim, eu mesmo criei os arquivos, caí nas armadilhas, debugei e venci!*
## 🏆 Nota:
*Para acelerar o domínio desta stack, utilizei o Gemini (IA) como tutor técnico e ferramenta de consulta. O foco foi validar conceitos de arquitetura, debugar erros de indentação em tempo real e simular cenários de deploy. Todo o código e documentação contidos aqui foram validados e implementados manualmente por mim, utilizando a IA para potencializar a curva de aprendizado.*
