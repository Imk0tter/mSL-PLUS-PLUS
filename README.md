## mSL++
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
  return Exception Caught on line $+($chr(40),%scriptLine,:,%scriptDir,$chr(41)) from Object ( $+ %object $+ : $+ $IsInstance(%object) $+ ): %error $+  - %message
}
;;;;;;;;;;;;;;;;;;;;;;;
; Main Class Function ;
;;;;;;;;;;;;;;;;;;;;;;;
alias ClassName {
  var %Class ClassName
  var %prop $mprop($prop)
  if !%prop {
    if ($IsPrivate(%Class,INIT)) {

      var %astart $iif($hget(MAKETOK, COUNT),$v1,0)
      maketok MAKETOK V %Class
      maketok MAKETOK V INIT
      var %aend $iif($hget(MAKETOK, COUNT),$v1,0)

      var %bstart $iif($hget(MAKETOK, COUNT),$v1,0)
      var %bend $iif($hget(MAKETOK, COUNT),$v1,0)

      var %cstart $iif($hget(MAKETOK,COUNT),$v1,0) + 1
      maketok MAKETOK V $*
      var %cend $iif($hget(MAKETOK,COUNT),$v1,0)

      var %object $meval(MAKETOK,%astart,%aend,%bstart,%bend,%cstart,%cend)
      - $inheritsFrom(%object,%Class)
      if ($hget(MAKETOK)) hfree MAKETOK
      return %object
    }
    if ($hget(MAKETOK)) hfree MAKETOK
  }
  else if $IsPublic(%class,$fprop(%prop)) {
    var %object $1

    var %astart $iif($hget(MAKETOK, COUNT),$v1,0)
    maketok MAKETOK V %Class
    maketok MAKETOK V $prop
    maketok MAKETOK V %object
    var %aend $iif($hget(MAKETOK, COUNT),$v1,0)

    var %bstart $iif($hget(MAKETOK, COUNT),$v1,0)
    var %bend $iif($hget(MAKETOK, COUNT),$v1,0)

    var %cstart $iif($hget(MAKETOK,COUNT),$v1,0) + 1
    maketok MAKETOK V $*
    var %cend $iif($hget(MAKETOK,COUNT),$v1,0)

    return $meval(MAKETOK,%astart,%aend,%bstart,%bend,%cstart,%cend)
  }
  else {
    if ($hget(MAKETOK)) hfree MAKETOK
    if $isinstance($1) {
      return $catch($1,MemberErr, $scriptline, $token($script,-1,92), $qt($fprop($mprop($prop))) is not a public member of class $qt(%class))
    }
    else {
      return $catch(%class,MemberErr, $scriptline, $token($script,-1,92), $qt($fprop($mprop($prop))) is not a public member of class $qt(%Class)).class  
    }
  }
}
;;;;;;;;;;;;;;;;;;;;;;;;;;;
; Initialization Function ;
;;;;;;;;;;;;;;;;;;;;;;;;;;;
alias -l ClassName.INIT {
  var %instance $iif($1,$1,$class)
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
;      var %astart $iif($hget(MAKETOK, COUNT),$v1,0)         ; For passing variables to the $meval function (or your own function)                      ;
;      maketok MAKETOK V ClassName                           ;                                                                                          ;
;      maketok MAKETOK V $prop                               ;                                                                                          ;
;      var %aend $iif($hget(MAKETOK, COUNT),$v1,0)           ;                                                                                          ;
;                                                            ;                                                                                          ;
;      var %bstart $iif($hget(MAKETOK, COUNT),$v1,0)         ; For passing variables before the $N- tokens are passed (any number of variables allowed  ;
;      maketok MAKETOK V SomeValue                           ;    these variables will be before the $N- tokens.                                        ;
;      maketok MAKETOK V SomeValue2                          ;                                                                                          ;
;      var %bend $iif($hget(MAKETOK, COUNT),$v1,0)           ;                                                                                          ;
;                                                            ;                                                                                          ;
;      var %cstart $iif($hget(MAKETOK,COUNT),$v1,0) + 1      ; Starting token number (if trying to pass $2- to a function you put + 2 instead of  +1    ; 
;      maketok MAKETOK V $*                                  ;    (which is for tokens $1-)                                                             ;
;      var %cend $iif($hget(MAKETOK,COUNT),$v1,0)            ;                                                                                          ;
;                                                           ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;             
;      return $meval(MAKETOK,%astart,%aend,%bstart,%bend,%cstart,%cend) ;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
alias -l meval {
  var %tokentable $1

  var %astart $2 + 1
  var %aend $3
  var %acount $calc(%aend - %astart + 1)

  var %bstart $4 + 1
  var %bend $5
  var %bcount $calc(%bend - %bstart + 1)

  var %cstart $6
  var %cend $$7
  var %ccount $calc(%cend - %cstart + 1)

  var %aregex $regsubex($str(.,%acount),/(.)/g,$+($,hget,$chr(40),%tokentable,$chr(44),$calc(\n + %astart - 1),$chr(41),$chr(58)))
  var %bregex $regsubex($str(.,%bcount),/(.)/g,$+($,hget,$chr(40),%tokentable,$chr(44),$calc(\n + %bstart - 1),$chr(41),$chr(58)))
  var %cregex $regsubex($str(.,%ccount),/(.)/g,$+($,hget,$chr(40),%tokentable,$chr(44),$calc(\n + %cstart - 1),$chr(41),$chr(58)))

  tokenize 58 %aregex

  var %class [ [ $1 ] ]
  var %prop [ [ $2 ] ]
  var %object [ [ $3 ] ]
  var %isObjectCall [ [ $4 ] ]
  
  if !%isObjectCall {
    if $isPublic(%class,$fprop($mprop(%prop))) || ($isPrivate(%class,$fprop($mprop(%prop))) && $fprop($mprop(%prop)) == INIT) {
      var %regex $+($,%class,.,$fprop($mprop(%prop)),$chr(40),$iif($fprop($mprop(%prop),2-),$v1 $+ $chr(44),$null $+ $chr(44)), $left($replace(%bregex %cregex,$chr(58),$chr(32) $chr(44) $chr(32)),-5),$chr(41),$iif($mprop(%prop,1),$+(.,$v1)))
      return [ [ %regex ] ]
    }
    else if $IsException(%class, $fprop($mprop(%prop))) {
      var %regex $+($,%class,.,EXCEPTION.,$fprop($mprop(%prop)),$chr(40),$iif($fprop($mprop(%prop),2-),$v1 $+ $chr(44),$null $+ $chr(44)), $left($replace(%bregex %cregex,$chr(58),$chr(32) $chr(44) $chr(32)),-5),$chr(41),$iif($mprop(%prop,1),$+(.,$v1)))
      return [ [ %regex ] ]
    }
  }
  else {
    var %regex $+($,OBJECT,$chr(40),$left($replace(%bregex %cregex,$chr(58),$chr(32) $chr(44) $chr(32)),-5),$chr(41),$iif(%prop,$+(.,$v1)))
    return [ [ %regex ] ]
  }
  if $isInstance(%object) {
    return $catch(%object,MemberErr, $scriptline, $token($script,-1,92), $qt($fprop($mprop(%prop))) is not a public member of %Class)
  }
  else {
    return $catch(%class, MemberErr, $scriptline, $token($script,-1,92), $qt($fprop($mprop(%prop))) is not a public member of %Class).class
  }
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
        var %astart $iif($hget(MAKETOK, COUNT),$v1,0)
        maketok MAKETOK V %currentClass
        maketok MAKETOK V $+(%error,.,$prop)
        maketok MAKETOK V %instanceOrClass
        var %aend $iif($hget(MAKETOK, COUNT),$v1,0)

        var %bstart $iif($hget(MAKETOK, COUNT),$v1,0)
        var %bend $iif($hget(MAKETOK, COUNT),$v1,0)

        var %cstart $iif($hget(MAKETOK,COUNT),$v1,0) + 1
        maketok MAKETOK V $*
        var %cend $iif($hget(MAKETOK,COUNT),$v1,0)
        return $meval(MAKETOK,%astart,%aend,%bstart,%bend,%cstart,%cend)
      }
      inc %x
    }
  }
  else if %isClass && $isClass(%instanceOrClass) {
    if $IsException(%instanceOrClass) {
      var %astart $iif($hget(MAKETOK, COUNT),$v1,0)
      maketok MAKETOK V %instanceOrClass $+ .EXCEPTION
      maketok MAKETOK V $+(%error,.,$mprop($prop,1))
      var %aend $iif($hget(MAKETOK, COUNT),$v1,0)

      var %bstart $iif($hget(MAKETOK, COUNT),$v1,0)
      var %bend $iif($hget(MAKETOK, COUNT),$v1,0)

      var %cstart $iif($hget(MAKETOK,COUNT),$v1,0) + 1
      maketok MAKETOK V $*
      var %cend $iif($hget(MAKETOK,COUNT),$v1,0)

      return $meval(MAKETOK,%astart,%aend,%bstart,%bend,%cstart,%cend)
    }
  }
  if ($hget(MAKETOK)) hfree MAKETOK
}
;;;;;;;;;;;;;
; End Catch ;
;;;;;;;;;;;;;
;;;;;;;;;;;;;;;;;;;;
; END CLASS FOOTER ;
;;;;;;;;;;;;;;;;;;;;
````

# $meval

To pass variables to the $meval function, call `maketok MAKETOK [V|B] [parameter]` between `%astart and %aend`

To pass variables to the fuction created by $meval, call `maketok MAKETOK [V|B] [parameter]`  between `%bstart and %bend`

To pass the current array of tokens to $meval, set `%cstart` to the current `$hget(MAKETOK, COUNT)` to be `+ x` where `x` is the number of the starting token


IE: `var %cstart $iif($hget(MAKETOK, COUNT),$v1,0) + 4` to start at the `$4`th token

IE: `var %cstart $iif($hget(MAKETOK, COUNT),$v1,0) + 5` to start at the `$5`th token

````
        var %astart $iif($hget(MAKETOK, COUNT),$v1,0)
        maketok MAKETOK V Class
        maketok MAKETOK V $prop
        maketok MAKETOK V %object
        maketok MAKETOK V $cprop(%params,IS_OBJECT_CALL)
        var %aend $iif($hget(MAKETOK, COUNT),$v1,0)

        var %bstart $iif($hget(MAKETOK, COUNT),$v1,0)
        ;maketok MAKETOK V %object
        var %bend $iif($hget(MAKETOK, COUNT),$v1,0)


        var %cstart $iif($hget(MAKETOK,COUNT),$v1,0) + 5
        maketok MAKETOK V $*
        var %cend $iif($hget(MAKETOK,COUNT),$v1,0)

        return $meval(MAKETOK,%astart,%aend,%bstart,%bend,%cstart,%cend)
````

Below is the `$meval` function and helper functions `maketok`, `$mprop`, `$fprop`, and `$cprop`

````
alias meval {
  var %tokentable $1

  var %astart $2 + 1
  var %aend $3
  var %acount $calc(%aend - %astart + 1)

  var %bstart $4 + 1
  var %bend $5
  var %bcount $calc(%bend - %bstart + 1)

  var %cstart $6
  var %cend $$7
  var %ccount $calc(%cend - %cstart + 1)

  var %aregex $regsubex($str(.,%acount),/(.)/g,$+($,hget,$chr(40),%tokentable,$chr(44),$calc(\n + %astart - 1),$chr(41),$chr(58)))
  var %bregex $regsubex($str(.,%bcount),/(.)/g,$+($,hget,$chr(40),%tokentable,$chr(44),$calc(\n + %bstart - 1),$chr(41),$chr(58)))
  var %cregex $regsubex($str(.,%ccount),/(.)/g,$+($,hget,$chr(40),%tokentable,$chr(44),$calc(\n + %cstart - 1),$chr(41),$chr(58)))

  tokenize 58 %aregex

  var %class [ [ $1 ] ]
  var %prop [ [ $2 ] ]
  var %object [ [ $3 ] ]
  var %isObjectCall [ [ $4 ] ]


  - CLASS: %class PROP: %prop OBJECT: %object FPROP: $fprop($mprop(%prop)) ISOBJECTCALL: %isObjectCall

  if !%isObjectCall {
    if $isPublic(%class,$fprop($mprop(%prop))) || ($isPrivate(%class,$fprop($mprop(%prop))) && ($fprop($mprop(%prop)) == INIT || $token(%class,2,46) == EXCEPTION))  {
      var %regex $+($,%class,.,$fprop($mprop(%prop)),$chr(40),$iif($fprop($mprop(%prop),2-),$v1 $+ $chr(44),$null $+ $chr(44)), $left($replace(%bregex %cregex,$chr(58),$chr(32) $chr(44) $chr(32)),-5),$chr(41),$iif($mprop(%prop,1),$+(.,$v1)))
      - REGEX1 %regex MAKETOK COUNT: $hget(MAKETOK,COUNT)
      return [ [ %regex ] ]
    }
  }
  else {
    var %regex $+($,OBJECT,$chr(40),$left($replace(%bregex %cregex,$chr(58),$chr(32) $chr(44) $chr(32)),-5),$chr(41),$iif(%prop,$+(.,$v1)))
    return [ [ %regex ] ]
  }
  if $isInstance(%object) {
    return $catch(%object,MemberErr, $scriptline, $token($script,-1,92), $qt($fprop($mprop(%prop))) is not a public member of %Class)
  }
  else {
    return $catch(%class, MemberErr, $scriptline, $token($script,-1,92), $qt($fprop($mprop(%prop))) is not a public member of %Class).class
  }
}
alias cprop {
  return $istok($1,$$2,58)
}
alias mprop {
  return $iif($2,$token($1,2-,46),$token($1,1,46))
}
alias fprop {
  return $token($1,$iif($2,$2,1),58)
}
alias maketok {
  hinc -m $1 COUNT
  hadd -m $+ $iif($2 == B,b) $1 $hget($1,COUNT) $3-
}
````




# Class Body

Below is body where all the public functions are implemented...


````
alias -l List.FunctionName {
  var %params $1
  var %object $2
  if $prop {
    var %astart $iif($hget(MAKETOK, COUNT),$v1,0)
    maketok MAKETOK V ClassName
    maketok MAKETOK V $prop
    maketok MAKETOK V %object
    maketok MAKETOK V $cprop(%params,IS_OBJECT_CALL)
    var %aend $iif($hget(MAKETOK, COUNT),$v1,0)

    var %bstart $iif($hget(MAKETOK, COUNT),$v1,0)
    ;maketok MAKETOK V %object
    var %bend $iif($hget(MAKETOK, COUNT),$v1,0)

    var %cstart $iif($hget(MAKETOK,COUNT),$v1,0) + 3
    maketok MAKETOK V $*
    var %cend $iif($hget(MAKETOK,COUNT),$v1,0)

    return $meval(MAKETOK,%astart,%aend,%bstart,%bend,%cstart,%cend)
  }
  if ($hget(MAKETOK)) hfree MAKETOK
}
````
