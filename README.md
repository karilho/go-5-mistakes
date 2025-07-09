## Aula: 5 Erros Comuns em Go para Iniciantes (Baseado no livro [100 Go Mistakes and How to Avoid Them](https://100go.co/))|

---

### Índice

1. [Código Muito Aninhado (Mistake #3: Deep Nesting)](https://100go.co/#unnecessary-nested-code-2)
2. [Confundir Paralelismo e Concorrência (Mistake #55: Mixing up concurrency and parallelism)](https://100go.co/mistakes/55)
3. [Confundir Slice `nil` com Slice Vazio (Mistake #22: Unexpected nil slices)](https://100go.co/#being-confused-about-nil-vs-empty-slice-22)
4. [Ignorar Erros de Funções (Mistake #53: Not handling an error)](https://100go.co/#not-handling-an-error-53)
5. [Ignorar que os Elementos São Copiados no `range` (Mistake #30: Ignoring that elements are copied in range loops)](https://100go.co/#ignoring-that-elements-are-copied-in-range-loops-30)

---

### 1. Código Muito Aninhado (Mistake #2: Deep Nesting)
**Problema:** Código muito aninhado torna a leitura difícil e pode esconder bugs.

**Exemplo ruim:**

```go
func verificarAcesso(idade int) string {
    if idade >= 0 {
        if idade >= 18 {
            return "Pode entrar"
        } else {
            return "Precisa ser maior de idade"
        }
    }
    return "Idade inválida"
}
```

**Exemplo melhor:**

```go
func verificarAcesso(idade int) string {
    if idade < 0 {
        return "Idade inválida"
    }
    if idade < 18 {
        return "Precisa ser maior de idade"
    }
    return "Pode entrar"
}
```

### 2. Confundir Paralelismo e concorrência (Mistake #55: Mixing up concurrency and parallelism)

**Problema:** Confundir concorrência com paralelismo

**Exemplo**

Concorrencia em Go = Brigam por determinado recurso, mas não necessariamente ao mesmo tempo. [DRIVETHRU]

Paralelismo em Go = Dividem tarefas para serem executadas ao mesmo tempo. [PEDÁGIO]
---


---

### 3. Confundir Slice `nil` com Slice Vazio (Mistake #22: Being confused about nil vs. empty slice)

**Problema:** Um slice `nil` é diferente de um slice vazio `[]`. Alguns sistemas tratam esses dois de forma diferente.

**Demonstração simples:**

```go
var a []int // slice nil
b := []int{} // slice vazio
fmt.Println(a == nil) // true
fmt.Println(len(b))   // 0
```

---

### 4. Ignorar Erros de Funções (Mistake #20: Ignoring errors)

**Problema:** Se você ignora erros, não sabe quando algo deu errado e o código pode continuar com valores inválidos.

**Ruim:**

```go
valor, _ := strconv.Atoi("abc") // erro ignorado
fmt.Println(valor) // imprime 0, mas com erro silencioso
```

**Correto:**

```go
valor, err := strconv.Atoi("abc")
if err != nil {
    fmt.Println("Erro ao converter string para número:", err)
    return
}
fmt.Println("Valor convertido:", valor)
```

---

### 5. Ignorar que os Elementos São Copiados no `range` (Mistake #30: Ignoring that elements are copied in range loops)

**Problema:** No `range`, cada item da lista é copiado para a variável de iteração. Isso pode causar comportamento inesperado ao modificar elementos ou trabalhar com ponteiros.

**Exemplo errado:**

```go
type Pessoa struct {
    Nome string
}

func main() {
    pessoas := []Pessoa{{"Ana"}, {"João"}, {"Bia"}}

    for _, p := range pessoas {
        p.Nome = "Alterado" // altera a cópia, não o original
    }

    fmt.Println(pessoas) // nomes continuam iguais
}
```

**Forma correta:**

```go
for i := range pessoas {
    pessoas[i].Nome = "Alterado"
}
fmt.Println(pessoas) // nomes alterados corretamente
```

**Alternativa usando `for` tradicional:**

```go
for i := 0; i < len(pessoas); i++ {
    pessoas[i].Nome = "Alterado"
}
fmt.Println(pessoas) // nomes alterados corretamente
```

---
