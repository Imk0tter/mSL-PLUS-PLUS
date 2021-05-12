## mSL++
Imk0tter's object oriented mSL implementation: mSL++


Below to create a new class, simply modify the first two instaces of 'ClassName' with your classes name.
````
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


alias ClassName.INIT {
  ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
  ; Below you call the constructor for the subclass of the current class. ;
  ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
  var %instance $Class                                        

  ; Do Initializing here ;
  ;;;;;;;;;;;;;;;;;;;;;;;;
  return %instance
}
````



Below is the main class of mSL++, which can be used to get and set variables.




````
;;;;;;;;;;;;;;;;;;;;;;;
; Public Declarations ;
;;;;;;;;;;;;;;;;;;;;;;;

alias Class.FREE.Public

alias Class.GET.Public

alias Class.SET.Public
alias Class.UNSET.Public

alias Class.EXPORT.Public
alias Class.IMPORT.Public
alias Class.CLONE.Public

;;;;;;;;;;;;;;;;;;;;;;;;;;
; Exception Declarations ;
;;;;;;;;;;;;;;;;;;;;;;;;;;

alias Class.EXCEPTION.Null {
  var %params $1, %object $2, %error $3, %message $6, %scriptLine $4, %scriptDir $5
  if ($hget(MAKETOK)) hfree MAKETOK
  return Exception Caught on line $+($chr(40),%scriptLine,:,%scriptDir,$chr(41)) from Object ( $+ %object $+ : $+ $IsInstance(%object) $+ ): %error $+  - %message
}
alias Class.EXCEPTION.NoOperation {
  var %params $1, %object $2, %error $3, %message $6, %scriptLine $4, %scriptDir $5
  if ($hget(MAKETOK)) hfree MAKETOK
  return Exception Caught on line $+($chr(40),%scriptLine,:,%scriptDir,$chr(41)) from Object ( $+ %object $+ : $+ $IsInstance(%object) $+ ): %error $+  - %message
}
alias Class.EXCEPTION.MemberErr {
  var %params $1, %object $2, %error $3, %message $6, %scriptLine $4, %scriptDir $5
  if ($hget(MAKETOK)) hfree MAKETOK
  return Exception Caught on line $+($chr(40),%scriptLine,:,%scriptDir,$chr(41)) from Object ( $+ %object $+ : $+ $IsInstance(%object) $+ ): %error $+  - %message
}

alias Class.EXCEPTION.ParamErr {
  var %params $1, %object $2, %error $3, %message $6, %scriptLine $4, %scriptDir $5
  if ($hget(MAKETOK)) hfree MAKETOK
  return Exception Caught on line $+($chr(40),%scriptLine,:,%scriptDir,$chr(41)) from Object ( $+ %object $+ : $+ $IsInstance(%object) $+ ): %error $+  - %message
}
alias Class.EXCEPTION.RangeErr {
  var %params $1, %object $2, %error $3, %message $6, %scriptLine $4, %scriptDir $5
  return Exception Caught on line $+($chr(40),%scriptLine,:,%scriptDir,$chr(41)) from Object ( $+ %object $+ : $+ $IsInstance(%object) $+ ): %error $+  - %message
}
;;;;;;;;;;;;;;;
; Class Alias ;
;;;;;;;;;;;;;;;

alias Class {
  var %class Class
  var %prop $mprop($prop)
  if !$mprop($prop) {
    if ($IsPrivate(%class,INIT)) {

      var %astart $iif($hget(MAKETOK, COUNT),$v1,0)
      maketok MAKETOK V Class
      maketok MAKETOK V INIT
      var %aend $iif($hget(MAKETOK, COUNT),$v1,0)

      var %bstart $iif($hget(MAKETOK, COUNT),$v1,0)
      var %bend $iif($hget(MAKETOK, COUNT),$v1,0)

      var %cstart $iif($hget(MAKETOK,COUNT),$v1,0) + 1
      maketok MAKETOK V $*
      var %cend $iif($hget(MAKETOK,COUNT),$v1,0)

      var %object $meval(MAKETOK,%astart,%aend,%bstart,%bend,%cstart,%cend)
      - $inheritsFrom(%object,%class)
      if ($hget(MAKETOK)) hfree MAKETOK
      return %object
    }
    if ($hget(MAKETOK)) hfree MAKETOK
  }
  else if $IsPublic(%class,$fprop($mprop($prop))) {
    var %object $1

    var %astart $iif($hget(MAKETOK, COUNT),$v1,0)
    maketok MAKETOK V Class
    maketok MAKETOK V $prop
    maketok MAKETOK V %object
    var %aend $iif($hget(MAKETOK, COUNT),$v1,0)

    var %bstart $iif($hget(MAKETOK, COUNT),$v1,0)
    ;;maketok MAKETOK V $fprop($mprop($prop), 2-)
    maketok MAKETOK V %object
    var %bend $iif($hget(MAKETOK, COUNT),$v1,0)

    var %cstart $iif($hget(MAKETOK,COUNT),$v1,0) + 2
    maketok MAKETOK V $*
    var %cend $iif($hget(MAKETOK,COUNT),$v1,0)

    return $meval(MAKETOK,%astart,%aend,%bstart,%bend,%cstart,%cend)
  }
  else {
    if ($hget(MAKETOK)) hfree MAKETOK
    if $isinstance($1) {
      return $catch($1,MemberErr, $scriptline, $token($script,-1,92), $qt($fprop($mprop($prop))) is not a public member of class $qt(%Class))
    }
    else {
      return $catch(%class,MemberErr, $scriptline, $token($script,-1,92), $qt($fprop($mprop($prop))) is not a public member of class $qt(%Class)).class  
    }
  }
}

;;;;;;;;;;;;;;;;;
; Class Methods ;
;;;;;;;;;;;;;;;;;
alias Class.INIT {
  var %params $1
  var %htable MSL++
  hinc -m %htable OBJECT_NUMBER
  var %object $+(%htable,_,$hget(%htable,OBJECT_NUMBER)) 

  hadd -m %htable $+(%object,_,INIT)
  return %object
}
alias Class.SET {
  var %params $1
  var %object $2
  var %variable $3
  var %value $$4

  if ($isInstance(%object)) {
    var %prop $mprop(%params)
    if $cprop(%prop,BVAR) || $cprop(%prop, B) {
      hadd -mb %object %variable %value

      if $prop {
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
      }
    }
    else {
      hadd -m %object %variable %value
      if $prop {
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
      }
    }
  }
  if ($hget(MAKETOK)) hfree MAKETOK
}

alias Class.GET {
  var %params $1
  var %object $2
  var %variableName $3
  var %bvar $4
  var %hget
  if $cprop(%params,BVAR) || $cprop(%params,B) {
    if $cprop(%params,ITEM) {
      if (%variableName isnum 1 - $hget(%object,0).item) {
        var %hget $hget(%object,%variableName, %bvar).item
      }
      else {
        if ($hget(MAKETOK)) hfree MAKETOK
        ;TODO: DO CATCH
        return $catch(%object,RangeErr,$scriptline, $token($script,-1,92),$qt(%variableName) is not a valid index for object $qt(%object) $+ . Valid Ranges: 1- $+ $hget(%object, 0).item)
      }
    }
    else {
      var %hget $hget(%object,%variableName,%bvar)
    }
    if $prop {
      var %astart $iif($hget(MAKETOK, COUNT),$v1,0)
      maketok MAKETOK V Class
      maketok MAKETOK V $prop
      maketok MAKETOK V %object
      maketok MAKETOK V $cprop(%params,IS_OBJECT_CALL)
      var %aend $iif($hget(MAKETOK, COUNT),$v1,0)

      var %bstart $iif($hget(MAKETOK, COUNT),$v1,0)
      ;maketok MAKETOK V %object
      $iif($cprop(%params,stack),maketok MAKETOK V %bvar)
      var %bend $iif($hget(MAKETOK, COUNT),$v1,0)

      var %cstart $iif($hget(MAKETOK,COUNT),$v1,0) + 5
      maketok MAKETOK V $*
      $iif(!$cprop(%params,stack), maketok MAKETOK V %bvar)
      var %cend $iif($hget(MAKETOK,COUNT),$v1,0)

      return $meval(MAKETOK,%astart,%aend,%bstart,%bend,%cstart,%cend)

    }
    if ($hget(MAKETOK)) hfree MAKETOK
    return %hget
  }
  else {
    var %hget
    if $cprop(%params, ITEM) {
      if (%variableName isnum 1 - $hget(%object,0).item) {
        var %hget $hget(%object,%variableName).item
      }
      else {
        ;TODO: DO_CATCH
        return $catch(%object,RangeErr,$scriptline, $token($script,-1,92),$qt(%variableName) is not a valid index for object $qt(%object) $+ . Valid Ranges: 1- $+ $hget(%object, 0).item)
      }
    }
    else {
      var %hget $hget(%object,%variableName)
    }
    if $prop {
      var %astart $iif($hget(MAKETOK, COUNT),$v1,0)
      maketok MAKETOK V Class
      maketok MAKETOK V $prop
      maketok MAKETOK V %object
      maketok MAKETOK V $cprop(%params,IS_OBJECT_CALL)
      var %aend $iif($hget(MAKETOK, COUNT),$v1,0)

      var %bstart $iif($hget(MAKETOK, COUNT),$v1,0)
      ;maketok MAKETOK V %object
      $iif($cprop(%params,STACK),maketok MAKETOK V %hget)
      var %bend $iif($hget(MAKETOK, COUNT),$v1,0)


      var %cstart $iif($hget(MAKETOK,COUNT),$v1,0) + 4
      maketok MAKETOK V $*
      $iif(!$cprop(%params,STACK), maketok MAKETOK V %hget)
      var %cend $iif($hget(MAKETOK,COUNT),$v1,0)

      return $meval(MAKETOK,%astart,%aend,%bstart,%bend,%cstart,%cend)
    }
    if ($hget(MAKETOK)) hfree MAKETOK
    return %hget
  }
}

alias Class.UNSET {
  var %params $1
  var %object $2
  ;;TODO CHECK FOR 'ITEM' IN PARAMS
  hdel %object $$3
  if $prop {
    var %astart $iif($hget(MAKETOK, COUNT),$v1,0)
    maketok MAKETOK V Class
    maketok MAKETOK V $prop
    maketok MAKETOK V %object
    maketok MAKETOK V $cprop(%params,IS_OBJECT_CALL)
    var %aend $iif($hget(MAKETOK, COUNT),$v1,0)


    var %bstart $iif($hget(MAKETOK, COUNT),$v1,0)
    ;maketok MAKETOK V %object
    var %bend $iif($hget(MAKETOK, COUNT),$v1,0)


    var %cstart $iif($hget(MAKETOK,COUNT),$v1,0) + 4
    maketok MAKETOK V $*
    var %cend $iif($hget(MAKETOK,COUNT),$v1,0)

    return $meval(MAKETOK,%astart,%aend,%bstart,%bend,%cstart,%cend)
  }
  if ($hget(MAKETOK)) hfree MAKETOK
}


alias Class_FREE_STACK {
  if !$hfind(CLASS_FREE_STACK, $1-) {
    hadd -m CLASS_FREE_STACK $1- 1
    return 1
  }
  return 0
}
alias Class.FREE {
  ;;;;;;;;;;;;;;;;;;;;;;
  ; Do Destroying here ;
  ;;;;;;;;;;;;;;;;;;;;;;
  ;;TODO CHECK FOR 'ITEM' IN PARAMS
  var %params $1
  var %object $2
  var %depth $3
  var %prop $mprop($prop)
  if $cprop(%params,ALL) || $cprop(%params,A) {
    if $hget(%object) {
      var %y $hget(%object,0).item
      while %y {
        var %currentObject $hget(%object, $hget(%object,%y).ITEM)

        var %astart $iif($hget(MAKETOK, COUNT),$v1,0)
        maketok MAKETOK V Class
        maketok MAKETOK V FREE:ALL
        maketok MAKETOK V %object
        maketok MAKETOK V $cprop(%params,IS_OBJECT_CALL)
        var %aend $iif($hget(MAKETOK, COUNT),$v1,0)

        var %bstart $iif($hget(MAKETOK, COUNT),$v1,0)
        maketok MAKETOK V %currentObject
        var %bend $iif($hget(MAKETOK, COUNT),$v1,0)

        var %cstart $iif($hget(MAKETOK,COUNT),$v1,0) + 3
        maketok MAKETOK V $*
        var %cend $iif($hget(MAKETOK,COUNT),$v1,0)

        if $CLASS_FREE_STACK(%currentObject) {
          - $meval(MAKETOK,%astart,%aend,%bstart,%bend,%cstart,%cend)
        }
        dec %y
      }
      if ($hget(CLASS_FREE_STACK)) hfree CLASS_FREE_STACK
    }
    hdel MSL++ %object $+ _INIT
    if ($hget(%object)) hfree %object 
  }
  else if $prop {
    var %astart $iif($hget(MAKETOK, COUNT),$v1,0)
    maketok MAKETOK V Class
    maketok MAKETOK V $prop
    maketok MAKETOK V %object
    maketok MAKETOK V $cprop(%params,IS_OBJECT_CALL)
    var %aend $iif($hget(MAKETOK, COUNT),$v1,0)

    var %bstart $iif($hget(MAKETOK, COUNT),$v1,0)
    maketok MAKETOK V %object
    var %bend $iif($hget(MAKETOK, COUNT),$v1,0)

    var %cstart $iif($hget(MAKETOK,COUNT),$v1,0) + 3
    maketok MAKETOK V $*
    var %cend $iif($hget(MAKETOK,COUNT),$v1,0)

    return $meval(MAKETOK,%astart,%aend,%bstart,%bend,%cstart,%cend)
  } 
  if ($hget(MSL++,%object $+ _INIT)) hdel MSL++ %object $+ _INIT
  if ($hget(%object)) hfree %object 
  if ($hget(MAKETOK)) hfree MAKETOK
}


;;;;;;;;;;;;;;;;;
; Class Methods ;
;;;;;;;;;;;;;;;;;

alias Class.IMPORT {
  var %params $1
  var %objectDir $mircdir $+ MSL++
  var %objectName $2
  if !$exists(%objectDir) { mkdir %objectDir }
  if $hget(%objectName) { return }
  hmake %objectName
  hload -b %objectName $+(%objectDir,\,%objectName,.obj)
  if $hget(%objectName) { 
    if $prop {
      var %astart $iif($hget(MAKETOK, COUNT),$v1,0)
      maketok MAKETOK V Class
      maketok MAKETOK V $prop
      maketok MAKETOK V %objectName
      maketok MAKETOK V $cprop(%params,IS_OBJECT_CALL)
      var %aend $iif($hget(MAKETOK, COUNT),$v1,0)

      var %bstart $iif($hget(MAKETOK, COUNT),$v1,0)
      maketok MAKETOK V %objectName
      var %bend $iif($hget(MAKETOK, COUNT),$v1,0)

      var %cstart $iif($hget(MAKETOK,COUNT),$v1,0) + 3
      maketok MAKETOK V $*
      var %cend $iif($hget(MAKETOK,COUNT),$v1,0)

      return $meval(MAKETOK,%astart,%aend,%bstart,%bend,%cstart,%cend)

    }
  }
  if $prop {
    var %astart $iif($hget(MAKETOK, COUNT),$v1,0)
    maketok MAKETOK V Class
    maketok MAKETOK V $prop
    maketok MAKETOK V %objectName
    maketok MAKETOK V $cprop(%params,IS_OBJECT_CALL)
    var %aend $iif($hget(MAKETOK, COUNT),$v1,0)

    var %bstart $iif($hget(MAKETOK, COUNT),$v1,0)
    maketok MAKETOK V %objectName
    var %bend $iif($hget(MAKETOK, COUNT),$v1,0)

    var %cstart $iif($hget(MAKETOK,COUNT),$v1,0) + 3
    maketok MAKETOK V $*
    var %cend $iif($hget(MAKETOK,COUNT),$v1,0)

    return  $meval(MAKETOK,%astart,%aend,%bstart,%bend,%cstart,%cend)
  }
  if ($hget(MAKETOK)) hfree MAKETOK
}

alias Class.EXPORT {
  var %params $1
  var %objectDir $mircdir $+ MSL++
  var %objectName $2
  if !$exists(%objectDir) { mkdir %objectDir }
  hsave -b %objectName $+(%objectDir,\,%objectName $+ .obj)
  if $prop {
    var %astart $iif($hget(MAKETOK, COUNT),$v1,0)
    maketok MAKETOK V Class
    maketok MAKETOK V $prop
    maketok MAKETOK V %objectName
    maketok MAKETOK V $cprop(%params,IS_OBJECT_CALL)
    var %aend $iif($hget(MAKETOK, COUNT),$v1,0)


    var %bstart $iif($hget(MAKETOK, COUNT),$v1,0)
    maketok MAKETOK V %objectName
    var %bend $iif($hget(MAKETOK, COUNT),$v1,0)

    var %cstart $iif($hget(MAKETOK,COUNT),$v1,0) + 3
    maketok MAKETOK V $*
    var %cend $iif($hget(MAKETOK,COUNT),$v1,0)

    return $meval(MAKETOK,%astart,%aend,%bstart,%bend,%cstart,%cend)
  }
  else {
    if ($hget(MAKETOK)) hfree MAKETOK
  }
}

alias Class.CLONE {
  var %params $1
  var %objectDir $mircdir $+ MSL++
  var %objectName $2
  if !$exists(%objectDir) { mkdir %objectDir }
  if $IsInstance(%objectName) {
    var %object $Class
    hsave -b %objectName $+(%objectDir,\,%objectName,.cpy)

    hload -mb %object $+(%objectDir,\,%objectName,.cpy)

    .remove $+(%objectDir,\,%objectName,.cpy)
    if ($hget(MAKETOK)) hfree MAKETOK
    return %object
  }
  if $prop {
    var %astart $iif($hget(MAKETOK, COUNT),$v1,0)
    maketok MAKETOK V Class
    maketok MAKETOK V $prop
    maketok MAKETOK V %objectName
    maketok MAKETOK V $cprop(%params,IS_OBJECT_CALL)
    var %aend $iif($hget(MAKETOK, COUNT),$v1,0)

    var %bstart $iif($hget(MAKETOK, COUNT),$v1,0)
    maketok MAKETOK V %objectName
    var %bend $iif($hget(MAKETOK, COUNT),$v1,0)

    var %cstart $iif($hget(MAKETOK,COUNT),$v1,0) + 3
    maketok MAKETOK V $*
    var %cend $iif($hget(MAKETOK,COUNT),$v1,0)

    return $meval(MAKETOK,%astart,%aend,%bstart,%bend,%cstart,%cend)
  }

  if ($hget(MAKETOK)) hfree MAKETOK
}

;;;;;;;;;;;;;
; End Class ;
;;;;;;;;;;;;;
````
