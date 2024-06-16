## Задание
Вариант 9: 
> Арифметическое “if”: <логическое выражение>? <выражение>: <выражение>.

  
 ## Реализация
1. Добавление токенов для ? и : (T_QUESTION, T_COLON)
2. Доработка функции `void Parser::statement()` в блоке `if(see(T_IDENTIFIER))`
3. Доработка `void  Parser::factor()` в блоке `else  if(match(T_LPAREN))`

Есть 2 пути решения:
**1 вариант реализации** - сделать аналогично функции `relation()`: после того, как пришел токен '(' вызывать `expression()` (аналогично базовой версии) и уже после этого проверять не появился ли токен сравнения:

```cpp
if(see(T_CMP)) {
    Cmp cmp = scanner_->getCmpValue();
    next();
    expression();
    switch(cmp) {
        //для знака "=" - номер 0
        case C_EQ:
            codegen_->emit(COMPARE, 0);
            break;
        //для знака "!=" - номер 1
        case C_NE:
            codegen_->emit(COMPARE, 1);
            break;
        //для знака "<" - номер 2
        case C_LT:
            codegen_->emit(COMPARE, 2);
            break;
        //для знака ">" - номер 3
        case C_GT:
            codegen_->emit(COMPARE, 3);
            break;
        //для знака "<=" - номер 4
        case C_LE:
            codegen_->emit(COMPARE, 4);
            break;
        //для знака ">=" - номер 5
        case C_GE:
            codegen_->emit(COMPARE, 5);
            break;
    };
}
```

**2 вариант реализации** - вызывать сразу `relation()`, т.к. внутри него содержится expression(), поэтому базовые примеры будут работать. Этот вариант показан в коде.


## Пример
Пример находится в файле *arithmetic_if.mil*
```
BEGIN
        i := 3;
        j := 2;
        
        max := (i > j) ? i : j;
        WRITE(max)
END
```