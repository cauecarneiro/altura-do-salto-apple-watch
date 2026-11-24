# Altura do salto com Apple Watch

Algoritmo em Swift que estima a altura de um salto com os sensores do Apple Watch, pensado para esportes como o futevôlei.

Projeto da Apple Developer Academy · watchOS 11.4+ · 2025

[SETE na App Store](https://apps.apple.com/br/app/sete-esporte-e-performance/id6751563867)

Este é o algoritmo usado no app SETE, publicado na App Store.

## O que faz
- Detecta a decolagem e a aterrissagem pela aceleração vertical do relógio (queda livre seguida de impacto).
- Mede o tempo de voo e estima a altura pela fórmula `h = g·t² / 8`.
- Valida cada salto com uma pontuação de 0 a 10, para descartar movimento de braço e falsos positivos.
- Mostra no relógio a altura do último salto e a do melhor salto.

## Meu papel
O SETE foi feito por um time de 5 na Apple Developer Academy. Este repositório é meu: todos os commits são meus, e o código do algoritmo está aqui.

## Como foi feito
- `Core/JumpDetector.swift` lê o Core Motion a 100 Hz, projeta a aceleração no eixo vertical e a suaviza com filtro de média móvel exponencial.
- A decolagem é uma sequência de amostras abaixo de 0,32 g, depois de um período estável; o pouso é um pico acima de 1,25 g.
- O instante de cruzamento é interpolado entre amostras, o que melhora o tempo de voo.
- A altura base recebe ajustes por faixa de altura e, quando o altímetro (barômetro) está disponível, mistura 80% do cálculo pelo acelerômetro com 20% da medida do barômetro.
- Parâmetros ficam em `Models/JumpConfiguration.swift`; a tela (`ContentView`) e o `JumpTrackerViewModel` seguem MVVM.
- Em iOS e macOS o detector é um stub vazio, para não quebrar a compilação.
- O guia de uso e de integração em outros projetos está em [`FutevoleiJumps/README.md`](FutevoleiJumps/README.md).

## Tecnologias
Swift, SwiftUI, Core Motion (acelerômetro e altímetro) e watchOS.

## Como rodar
1. Abra `FutevoleiJumps/FutevoleiJumps.xcodeproj` no Xcode.
2. Rode o alvo `FutevoleiJumps Watch App` num Apple Watch de verdade (watchOS 11.4 ou mais novo), porque o simulador não tem sensores de movimento reais.
3. Para usar em outro projeto, copie `JumpDetector.swift`, `JumpConfiguration.swift` e `JumpData.swift` e adicione `NSMotionUsageDescription` ao `Info.plist`.

## Aprendizados
- Só o tempo de voo já dá uma altura, mas o código dedica boa parte do esforço a validar o salto (estabilidade antes, impacto na queda e pontuação) para evitar falsos positivos.

---
Feito por Cauê Crillanovick · [portfólio](https://cauecarneiro.github.io/portfolio/) · [LinkedIn](https://www.linkedin.com/in/cau%C3%AA-carneiro)
