# Como utilizar um RPA

- Logar no site
- Preencher formulário
- Receber os dados finais

- Atualizar site

```browser.refresh()
browser.refresh()
```

- Fazer login

```html

<html>
<body>
<form id="loginForm">
    <input name="username" type="text"/>
    <input name="password" type="password"/>
    <input name="continue" type="submit" value="Login"/>
</form>
</body>
</html>
```

## Pegando todos as tags de href

```
elems = driver.find_elements_by_xpath("//a[@href]")
for elem in elems:
    print elem.get_attribute("href")
```

## Pegar url

````python
url = driver.current_url
````

## Inserindo dados em um input

- HTML que iremos utilizar como exemplo

```html
<input type="text" name="passwd" id="passwd-id"/>
```

- Selecionando o campo pelo seu id

```python
element = driver.find_element_by_id("passwd-id")
```

- Inserindo string para

```python
element.send_keys("Adicionar Texto")
```

Selecionando tag **A**:

````html
<a href="https://www.be3.co/">Be3 soluções</a>
````

```python
driver.find_element_by_partial_link_text('Be3 soluções')
```

## Clicar em um botão com script

- sobre o erro:  `element click intercepted: Element Other element would receive the click:`

```python
driver.execute_script("arguments[0].click();", element)
```

## Trocar atributo do elemento

```python
driver.execute_script("arguments[0].setAttribute('value',arguments[1])", cbo, json_data['cbo'])
```

## Xpath para qualquer elemento da tag

- exemplo com botão e `data-bind`

````python
driver.find_element(By.XPATH, '//button[@data-bind="click: salvar, enable: !isSaving()"]')
````

## Pegando um ou vários elementos

```python
from selenium.webdriver.common.by import By

driver.find_element(By.XPATH, '//button[text()="Some text"]')
driver.find_elements(By.XPATH, '//button')
```

## Páginas com `iframe:`

[Movendo-se entre janelas e frames](https://selenium-python.readthedocs.io/navigating.html#moving-between-windows-and-frames)

Existem site que carregam HTML dentro de HTML com **iframes** e é necessário acessar o conteúdo dentro deles:

```python
referencia = driver.find_element_by_name(name)
driver.switch_to_frame(referencia)
# Sair do frame 
driver.switch_to.default_content()
```

## Capturando CSS de um elemento

```python
driver.find_element_by_id("passwd-id").value_of_css_property("color")
```

## Páginas com `select`:

[(Documentação) Preenchendo formulários](https://selenium-python.readthedocs.io/navigating.html#filling-in-forms)

Como selecionar um elemento especifico de um `select`

- Solução para apenas uma opção

```python
# "//select[@TAG='VALOR DA TAG']/option[text()='OPÇÃO PARA SELECIONAR']"
driver.find_element_by_xpath("//select[@name='sgl_uf_conselho']/option[text()='SP']").click()
```

- Comparando valor do value

```python
element = driver.find_element_by_xpath("//select[@name='name']")
all_options = element.find_elements_by_tag_name("option")
for option in all_options:
    print("Value is: %s" % option.get_attribute("value"))
    option.click()
```

- Comparando valor do texto

```python
element = driver.find_element_by_xpath('//select[@title="Selecione aqui o tipo de atendimento."]')
all_options = tipo_atendimento.find_elements(By.TAG_NAME, "option")
for option in all_options:
    if json_data['tipoAtendimento'].upper() in option.text.upper():
        option.click()
        break
```

- Selecionando por index

```python
select = Select(driver.find_element(By.ID, 'campo_id'))
select.select_by_index(1)
```

- Selecionar por texto visivel

````python
select = Select(driver.find_element(By.ID, 'Id_campo')).options
for item in select:
    if json_data['campo'].lower() in item.text.lower():
        select.select_by_visible_text(item.text)
````

- Selecionar por value

````python
select = Select(driver.find_element(By.ID, 'Id'))
select.select_by_value('2')
````

## Páginas que abrem outra `janela`:

[Movendo-se entre janelas e frames](https://selenium-python.readthedocs.io/navigating.html#moving-between-windows-and-frames)

```python
# Janela principal
driver.switch_to_window(driver.window_handles[0])

# Outra janela
driver.switch_to_window(driver.window_handles[1])

# funções nessa nova janela ..

# voltando para janela principal
driver.switch_to_window(driver.window_handles[0])
```

## Verificando alert

```python
# Verifica se tem algum alert
alert = driver.switch_to.alert
# Captura o texto do alert
erro = alert.text
# Clica no alert
alert.accept()
```

```
```

## Clicando em dois botões:

[Documentação ActionChains](https://selenium-python.readthedocs.io/api.html#selenium.webdriver.common.action_chains.ActionChains)

```python
from selenium.webdriver import ActionChains
from selenium.webdriver.support import expected_conditions as EC

action = ActionChains(driver)
# Clicando no (ALT + S)
action.key_down(Keys.ALT).send_keys('S').perform()
```

## Executar códigos JS com selenium

[`execute_script`( *
script* , ** args* )](https://selenium-python.readthedocs.io/api.html#selenium.webdriver.remote.webdriver.WebDriver.execute_script)

````python
driver.execute_script('alert("alert via selenium")')
````

# Waits

[Documentação sobre Waits](https://selenium-python.readthedocs.io/waits.html)

````python
# Aguarda até o elemento ficar visível na tela
WebDriverWait(driver, 10).until(EC.visibility_of_element_located((By.NAME, 'name_do_elemento')))

# Aguarda até o elemento com certo ID ser clicável
WebDriverWait(driver, 5).until(EC.element_to_be_clickable((By.ID, 'id_do_elemento')))

# Aguarda até o elemento aparecer no corpo HTML 
WebDriverWait(driver, 3).until(EC.presence_of_element_located((By.CLASS_NAME, 'classe_do_elemento')))
````
