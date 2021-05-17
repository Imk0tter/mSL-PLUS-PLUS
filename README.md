# mSL++
Imk0tter's object oriented mSL implementation: mSL++

# Class Header

The class header is to be modified for every class; simply implement your own public functions and change the 'ClassName' to the name of your class.

````
;;;;;;;;;;;;;;;;
; Class Header ;
;;;;;;;;;;;;;;;;

;;;;;;;;;;;;;;;;;;;;;;;
; Public Declarations ;
;;;;;;;;;;;;;;;;;;;;;;;
alias ClassName.FunctionName.Public
;;;;;;;;;;;;;;;;;;;;;;;;;;
; Exception Declarations ;
;;;;;;;;;;;;;;;;;;;;;;;;;;
alias ClassName.EXCEPTION.ExceptionName {
  var %params $1, %object $2, %error $3, %message $6, %scriptLine $4, %scriptDir $5
  + Exception Caught on line $+($chr(40),%scriptLine,:,%scriptDir,$chr(41)) from Object ( $+ %object $+ : $+ $IsInstance(%object) $+ ): %error $+  - %message
  return $null
}
;;;;;;;;;;;;;;;;;;;;;;;
; Main Class Function ;
;;;;;;;;;;;;;;;;;;;;;;;
alias ClassName {
  var %Class ClassName
  var %prop $mprop($prop)
  if !%prop {
    if ($IsPrivate(%Class,INIT)) {

      var %astart $MAKETOKCOUNT
      MAKETOK %Class
      MAKETOK MAKETOK V INIT
      var %aend $MAKETOKCOUNT

      var %bstart $MAKETOKCOUNT
      var %bend $MAKETOKCOUNT

      var %cstart $MAKETOKCOUNT + 1
      MAKETOK $*
      var %cend $MAKETOKCOUNT

      var %dstart $MAKETOKCOUNT
      ; For what ever comes after the tokens
      var %dend $MAKETOKCOUNT
      
      var %object $meval(%astart,%aend,%bstart,%bend,%cstart,%cend,%dstart,%dend)
      - $inheritsFrom(%object,%Class)
      UNMAKETOK
      return %object
    }
    UNMAKETOK
  }
  else if $IsPublic(%class,$fprop(%prop)) {
    var %object $1

    var %astart $MAKETOKCOUNT
    MAKETOK %Class
    MAKETOK $prop
    MAKETOK %object
    var %aend $MAKETOKCOUNT

    var %bstart $MAKETOKCOUNT
    var %bend $MAKETOKCOUNT

    var %cstart $MAKETOKCOUNT + 1
    MAKETOK $*
    var %cend $MAKETOKCOUNT
    
    var %dstart $MAKETOKCOUNT
    ; For what ever comes after the tokens
    var %dend $MAKETOKCOUNT

    return $meval(%astart,%aend,%bstart,%bend,%cstart,%cend,%dstart,%dend)
  }
  else {
    UNMAKETOK
    if $isinstance($1) {
      return $catch($1,MemberErr, $scriptline, $token($token($script,-1,92),1,46), $qt($fprop($mprop($prop))) is not a public member of class $qt(%class))
    }
    else {
      return $catch(%class,MemberErr, $scriptline, $token($token($script,-1,92),1,46), $qt($fprop($mprop($prop))) is not a public member of class $qt(%Class)).class  
    }
  }
}
;;;;;;;;;;;;;;;;;;;;;;;;;;;
; Initialization Function ;
;;;;;;;;;;;;;;;;;;;;;;;;;;;
alias -l ClassName.INIT {
  var %params $1
  var %object $2
  var %instance $iif(%object,%object,$class)
  - $class(%instance,COUNT,0).set
  - $class(%instance,TOTAL,0).set
  ;;;;;;;;;;;;;;;;;;;;;;;;
  ; Do Initializing here ;
  ;;;;;;;;;;;;;;;;;;;;;;;;
  return %instance
}
;;;;;;;;;;;;;;;;;;;;
; END CLASS HEADER ;
;;;;;;;;;;;;;;;;;;;;
````
# Class Body

Below is body where all the public functions are implemented...


````
;;;;;;;;;;;;;;
; Class Body ;
;;;;;;;;;;;;;;
alias -l ClassName.FunctionName {
  var %params $1
  var %object $2
  
  ;;;;;;;;;;;;;;;;;;;;;;;;;;;;
  ; Function code goes below ;
  ;;;;;;;;;;;;;;;;;;;;;;;;;;;;
  
  if $IsInstance(%object) {
    return $catch(ClassName,ExceptionName,$scriptline,$token($token($script,-1,92),1,46),there is no object specified for function $qt(FunctionName)).class
  }
  else {
    retrun $catch(%object, Null, $scriptline, $token($token($script,-1,92),1,46), the function $qt(FunctionName) of class $token($token($script,-2,92),1,46) has not yet been implemented!)
  }
  
  ;;;;;;;;;;;;;;;;;;;;;
  ; End Function Code ;
  ;;;;;;;;;;;;;;;;;;;;;
  
  if $prop {
    var %astart $MAKETOKCOUNT
    MAKETOK ClassName
    MAKETOK $prop
    MAKETOK %object
    MAKETOK $cprop(%params,IS_OBJECT_CALL)
    var %aend $MAKETOKCOUNT

    var %bstart $MAKETOKCOUNT
    ;MAKETOK %object
    var %bend $MAKETOKCOUNT

    var %cstart $MAKETOKCOUNT + 3
    MAKETOK $*
    var %cend $MAKETOKCOUNT
    
    var %dstart $MAKETOKCOUNT
    ; For what ever comes after the tokens
    var %dend $MAKETOKCOUNT

    return $meval(%astart,%aend,%bstart,%bend,%cstart,%cend,%dstart,%dend)
  }
  UNMAKETOK
}
;;;;;;;;;;;;;;;;;;
; End Class Body ;
;;;;;;;;;;;;;;;;;;
````

# Class Footer

The class footer is to be appended to the end of every class.

````
;;;;;;;;;;;;;;;;
; Class Footer ;
;;;;;;;;;;;;;;;;
alias -l - { !noop $1- }
alias -l + { $iif($Window(@Debug),echo @Debug,!noop) $iif($1-,$v1,$crlf) }

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
; Description: Returns whether or not a class member ;
; is private.                                        ;
;                                                    ;
; Usage: $IsPrivate(<Class>,<Member>)                ;
; Example: if ($IsInstanceOf(%Player,Player)) ..     ;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
alias -l IsPrivate return $iif($IsClass($1) && $isalias($+($1.,$2)),$true,$false)

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
; Description: Returns whether or not a class member ;
; is an exception.                                   ;
;                                                    ;
; Usage: $IsException(<Class>,<Member>)              ;
; Example: if ($IsExceptiion(%class,Null)) ..        ;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
alias -l IsException return $iif($isalias($+($1,.EXCEPTION.,$$2)),$true,$false)

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
; Code for passing dynamic variables to the $meval function ;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;      var %astart $MAKETOKCOUNT         ; For passing variables to the $meval function (or your own function)                      ;
;      MAKETOK ClassName                           ;                                                                                          ;
;      MAKETOK $prop                               ;                                                                                          ;
;      var %aend $MAKETOKCOUNT           ;                                                                                          ;
;                                                            ;                                                                                          ;
;      var %bstart $MAKETOKCOUNT         ; For passing variables before the $N- tokens are passed (any number of variables allowed  ;
;      MAKETOK SomeValue                           ;    these variables will be before the $N- tokens.                                        ;
;      MAKETOK SomeValue2                          ;                                                                                          ;
;      var %bend $MAKETOKCOUNT           ;                                                                                          ;
;                                                            ;                                                                                          ;
;      var %cstart $MAKETOKCOUNT + 1      ; Starting token number (if trying to pass $2- to a function you put + 2 instead of  +1    ; 
;      MAKETOK $*                                  ;    (which is for tokens $1-)                                                             ;
;      var %cend $MAKETOKCOUNT            ;                                                                                          ;
;                                                           ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;             
;      return $meval(%astart,%aend,%bstart,%bend,%cstart,%cend) ;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
alias -l meval {
  var %astart $1
  var %aend $2
  var %acount %aend - %astart

  var %bstart $3
  var %bend $4
  var %bcount %bend - %bstart

  var %cstart $5 - 1
  var %cend $$6
  var %ccount %cend - %cstart

  var %dstart $7
  var %dend $8
  var %dcount %dend - %dstart

  var %localVariables

  var %x 1
  while %x <= %acount {
    var %y %x + %astart
    var %localVariables %localVariables $+ $chr(255) $+ $ $+ getmaketok $+ [ ( ] $+ %y $+ [ ) ]
    inc %x
  }

  tokenize 255 %localVariables

  var %class [ [ $1 ] ]
  var %prop [ [ $2 ] ]
  var %object [ [ $3 ] ]
  var %isObjectCall [ [ $4 ] ]

  var %header,%footer,%body

  var %x 1
  while %x <= %bcount {
    var %y %x + %bstart
    var %header %header $+ $chr(255) $+ $ $+ getmaketok $+ [ ( ] $+ %y $+ [ ) ]
    inc %x
  }

  var %x 1
  while %x <= %ccount {
    var %y %x + %cstart
    var %body %body $+ $chr(255) $+ $ $+ getmaketok $+ [ ( ] $+ %y $+ [ ) ]
    inc %x
  }

  var %x 1
  while %x <= %dcount {
    var %y %x + %dstart
    var %footer %footer $+ $chr(255) $+ $ + getmaketok $+ [ ( ] $+ %y $+ [ ) ]
  }

  var %eval,%endeval

  ;;var %eval $ $+ %class $+ . $+ $fprop($mprop(%prop)) $+ [ ( ] $+ [ $fprop($mprop(%prop),2-) ] $+ [ , ]
  ;;var %endeval [ ) ] $+ . $+ [ $mprop(%prop,1) ]

  ;;;;;;;;;;;;;;;
  if !%isObjectCall {
    if $isPublic(%class,$fprop($mprop(%prop))) || ($isPrivate(%class,$fprop($mprop(%prop))) && $fprop($mprop(%prop)) == INIT) {
      var %eval $ $+ %class $+ . $+ $fprop($mprop(%prop)) $+ [ ( ] $+ $fprop($mprop(%prop),2-) $+ [ , ]
      var %endeval [ ) ] $+ . $+ [ $mprop(%prop,1) ]
    }
    else if $IsException(%class, $fprop($mprop(%prop))) {
      var %eval $ $+ %class $+ .EXCEPTION. $+ $fprop($mprop(%prop)) $+ [ ( ] $+ $fprop($mprop(%prop),2-) $+ [ , ]
      var %endeval [ ) ] $+ . $+ $mprop(%prop,1)
    }
    else {
      if ($IsInstance(%obejct))  {
        UNMAKETOK
        return $catch(%object, NoOperation, $scriptline, $token($token($script,-1,96),1,46), $qt($fprop($mprop(%prop))) is not a public member of class $qt(%class))
      }
      else {
        UNMAKETOK
        return $catch(Class, NoOperation, $scriptline, $token($token($script,-1,96),1,46), $qt($fprop($mprop(%prop))) is not a public member of class $qt(%class)).class
      }
    }
  }
  else {
    var %eval $ $+ OBJECT $+ [ ( ]
    var %endeval [ ) ] $+ . $+ $prop
  }
  ;;;;;;;;;;;;;;;;;

  tokenize 255 %header $+ %body $+ %footer

  var %x 1
  while %x <= $0 {
    if %x == $0 {
      var %eval %eval $+ [ $ $+ [ %x ] ]
    }
    else {
      var %eval %eval $+ [ $ $+ [ %x ] ] $+ [ , ]
    }
    inc %x
  }  
  return [ [ %eval ] $+ [ %endeval ] ]
}
;;;;;;;;;;;;;
; End meval ;
;;;;;;;;;;;;;

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
; Description: Called when ever an error is caught   ;
;                                                    ;
; Usage: $catch(<Instance>,<Error>,<Message>)        ;
; Example: if (!$IsInstanceOf(%Player,Player)) {     ;
; $catch(%Player,InstanceErr,Object %player is not   ;
;  an instance of class Player)                      ;
; }                                                  ;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
alias -l CATCH {
  var %error $2,%message $5,%instanceOrClass $1,%scriptLine $3, %scriptDir $4, %isClass $iif($mprop($prop) == class, $true,$false)

  if $isInstance(%instanceOrClass) && !%isClass {
    var %x 1
    var %inheritance $hget(MSL++,$+(%instanceOrClass,_,INIT))
    while $token(%inheritance,%x,32) {
      var %currentClass $v1
      if $IsException(%currentClass,%error) {
        var %astart $MAKETOKCOUNT
        MAKETOK %currentClass
        MAKETOK $+(%error,:,$token($token($script,-1,92),1,46),.,$prop)
        MAKETOK %instanceOrClass
        var %aend $MAKETOKCOUNT

        var %bstart $MAKETOKCOUNT
        var %bend $MAKETOKCOUNT

        var %cstart $MAKETOKCOUNT + 1
        MAKETOK $*
        var %cend $MAKETOKCOUNT

        var %dstart $MAKETOKCOUNT
        ; For what ever comes after the tokens
        var %dend $MAKETOKCOUNT

        return $meval(%astart,%aend,%bstart,%bend,%cstart,%cend,%dstart,%dend)
      }
      inc %x
    }
  }
  else if %isClass && $isClass(%instanceOrClass) {
    if $IsException(%instanceOrClass,%error) {
      var %astart $MAKETOKCOUNT
      MAKETOK %instanceOrClass
      MAKETOK $+(%error,:,$token($token($script,-1,92),1,46),.,$mprop($prop,1))
      var %aend $MAKETOKCOUNT

      var %bstart $MAKETOKCOUNT
      var %bend $MAKETOKCOUNT

      var %cstart $MAKETOKCOUNT + 1
      MAKETOK $*
      var %cend $MAKETOKCOUNT

      var %dstart $MAKETOKCOUNT
      ; For what ever comes dynamic set of paramters that come after the tokens
      var %dend $MAKETOKCOUNT

      return $meval(%astart,%aend,%bstart,%bend,%cstart,%cend,%dstart,%dend)
    }
  }
  UNMAKETOK
}
;;;;;;;;;;;;;
; End Catch ;
;;;;;;;;;;;;;
;;;;;;;;;;;;;;;;;;;;
; END CLASS FOOTER ;
;;;;;;;;;;;;;;;;;;;;
````

# $meval

To pass variables to the $meval function, call `maketok [parameter]` between `%astart and %aend`

To pass variables to the fuction created by $meval, call `maketok [parameter]`  between `%bstart and %bend`

To pass the current array of tokens to $meval, set `%cstart` to the current `$MAKETOKCOUNT` to be `+ x` where `x` is the number of the starting token


IE: `var %cstart $MAKETOKCOUNT + 4` to start at the `$4`th token

IE: `var %cstart $MAKETOKCOUNT + 5` to start at the `$5`th token

````
var %astart $MAKETOKCOUNT
MAKETOK Class
MAKETOK $prop
MAKETOK %object
MAKETOK $cprop(%params,IS_OBJECT_CALL)
var %aend $MAKETOKCOUNT

var %bstart $MAKETOKCOUNT
MAKETOK FIRSTVAR
MAKETOK SECONDVAR
MAKETOK THIRDVAR
var %bend $MAKETOKCOUNT


var %cstart $MAKETOKCOUNT + 1
MAKETOK $*
var %cend $MAKETOKCOUNT

var %dstart $MAKETOKCOUNT
MAKETOK extravar1
MAKETOK extravar2
var %dend $MAKETOKCOUNT

return $meval(%astart,%aend,%bstart,%bend,%cstart,%cend,%dstart,%dend)
````

# $catch

The catch function is used to handle errors in your classes. You call $catch where ever there's an error in your class.

````
if (!$IsInstance(%object)) {
return $catch(%object, Null, $scriptline, $token($token($script,-1,92),1,46), the object passed to this function is not an object!)
}
else if (%object) {
return $catch(ClassName,ExceptionName,$scriptline,$token($token($script,-1,92),1,46),there is no object specified for function $qt(FunctionName)).class
}
else {
;; Put your function code here ;;
}
````

# $object

The object function calls the first found instance of the specified function in the class heirarchy.

````
var %object $Class
+ $Object(%object,variable,value).set
+ $Object(%object,variable).get

+ $Class(%object,variable,value).get
+ $Class(%object, variable).get

var %object2 $List
+ $Object(%object2,value).add
+ $Object(%object2,value2).add

+ $List(%object2, value3).add
+ $List(%object2, value4).add
````

