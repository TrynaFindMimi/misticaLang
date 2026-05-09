# gramatica
grammar edu.upb.lp.isc.MisticaLang with org.eclipse.xtext.common.Terminals

generate misticaLang "http://www.upb.edu/lp/isc/MisticaLang"

Model:
    ( 'Veamos' | 'veamos' ) ' que dicen las cartas acerca de'  name=ID  ( 'deja canalizo tu energia' | 'necesito saber lo siguiente' expr=Expression )
    statements+=Statement*
    ( ( 'Si' | 'si' ) ' ya no tienes nada mas que preguntar pasemos a la siguiente pregunta' 
    | ( 'Si' | 'si' ) ' eso fue todo nos vemos una proxima vez' )
;

Statement:
    Oraculo | Arcano | Pendulo | Revelation | WhileLoop | Conditional | Scanner | Assignment
;

Variable:
	Arcano | Oraculo | Pendulo
;

// Declaración de String
Oraculo:
    ( 'El' | 'el' ) ' oraculo de' name=ID 'nos indica que' value=STRING |
    ( 'El' | 'el' ) ' oraculo de' name=ID 'guarda silencio'
;

// Declaración de Int
Arcano:
    ( 'Surge' | 'surge' ) ' el arcano' value=INT 'de ' name=ID |
    ( 'Surge' | 'surge' ) ' un arcano' name=ID 'sin voltear'
;

// Declaración de Boolean
Pendulo:
    ( 'El' | 'el' ) ' pendulo de la' name=ID 'se inclina hacia el' value=BooleanValue |
    ( 'El' | 'el' ) ' pendulo de el' name=ID 'se inclina hacia un' value=BooleanValue
;

BooleanValue:
    ( 'si' | 'Si' ) | ( 'no' | 'No' )
;

// Para imprimir
Revelation:
    ('Las'|'las')'energias se canalizan y nos revelan que' message=RevelationMessage
;

RevelationMessage:
    parts+=Expression( ( 'union con' | 'se une a' | 'es acompañado por' |'abraza a ') parts+=Expression )*
;

// While loop
WhileLoop:
    ( ( 'Se' | 'se' ) ' barajea hasta' | ( 'Mientras' | 'mientras' ) ' se busca que' ) condition=Condition
        statements+=Statement*
    ( 'Se' | 'se' ) ' llega al equilibrio y se pide un consejo'
;

// Condicional if/elseif/else
Conditional:
    ( 'Saquemos' | 'saquemos' ) ' las primeras cartas ' ifBranch=IfBranch (elseIfBranches+=ElseIfBranch)* (elseBranch=ElseBranch)?
    ( 'Ese' | 'ese' ) ' fue el mensaje de las cartas'
;

IfBranch:
    ( ( 'El' | 'el' ) ' sol brilla si' | ( 'La' | 'la' ) ' energia del pasado me muestra' ) condition=Condition
        statements+=Statement*
;

ElseIfBranch:
    ( ( 'El' | 'el' ) ' juicio evalua si' | ( 'Tu' | 'tu' ) ' energia vibra con ' | ( 'El' | 'el' ) ' presente me indica que ' ) condition=Condition
        statements+=Statement*
;

ElseBranch:
    ( ( 'La' | 'la' ) ' luna ilumina' | ( 'El' | 'el' ) ' futuro resuena a ' )
        statements+=Statement*
;

// Condición ==
Condition:
    left=Expression ( 'armoniza' | 'alcance' | 'se opone' | ( 'y' | 'Y' ) | ( 'o' | 'O' ) | 'es mas fuerte que' | 'es mas debil que' ) right=Expression
    //==  = != > <
;

Expression:
    IntValue | StringValue | BooleanValue | Operations | VariableRef
;

VariableRef:
	var=[Variable]
;

IntValue:
	int=INT
;

StringValue:
	string=STRING
;

Operations:
    left=Primary 'se une' right=Primary
  | left=IntValue 'pierde' right=IntValue
  | left=IntValue 'crece gracias a' right=IntValue
  | left=IntValue 'se reparte entre' right=IntValue
  | left=IntValue 'se invierte' right=IntValue
;

Primary:
    IntValue | StringValue | VariableRef
;

Assignment:
    var=[Variable] 'surge un cambio a' newValue=Expression
;

// Scanner
Scanner:
    ( 'La' | 'la' ) ' energia requiere'
        var=[Variable];
