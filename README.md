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
