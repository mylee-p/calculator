## Calculator [계산기 개발]

HTML, CSS, Javascript 만으로 단순하게 개발하기

<img width="248" alt="calculator" src="https://user-images.githubusercontent.com/89143892/211467882-38ead60c-8f61-4601-8072-9f75df5bade4.png">

<br />

### 두가지 숫자를 받아서 연산자를 활용해 계산하여 값이 나올 수 있도록 구현

1. 아무것도 입력되지 않은 input
2. 결과값이 입력된 input
3. 덧셈 등 연산을 진행하면 + 등 연산자가 생기는 input
4. 숫자를 입력하면 숫자가 바로 입력되는 input
5. 뒤에 입력한 숫자가 음수일 경우 +연산자가 -연산자로 바뀌지 않고 음수가 입력되는 input
6. \*를 하다가 뒤에 음수가 올 수 있기 때문에 5번과 같은 input
7. =를 입력하면 바로 연산된 값이 보이는 input
8. 연산된 값이 있는 상태에서 Enter를 입력하면 연산수식없이 결과값만 남는 상태의 input
9. =을 입력한 생태에서 Enter가 아닌 다른 값을 입력하면 리셋 시켜서 해당 값을 넣은 상태로 <br/>출력되는 input

<br/>

### 간단한 HTML 구조만들기

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        . .
        <script src="main.js"></script>
        <title>Calculator</title>
    </head>
    <body>
        <div class="row">
            <!-- input을 컨트롤 해야하기 때문에 id를 만든다. -->
            <input class="inp" disabled id="top-inp" />
        </div>
        <div class="row">
            <!-- inputNum 함수를 onClick 이벤트에 넣어서 적용시킨다. -->
            <div class="btn" onClick="inputNum(7)">7</div>
            <div class="btn" onClick="inputNum(8)">8</div>
            <div class="btn" onClick="inputNum(9)">9</div>
            <div class="btn" onClick="inputEqu('=')">=</div>
        </div>
        <div class="row">
            <div class="btn" onClick="inputNum(4)">4</div>
            <div class="btn" onClick="inputNum(5)">5</div>
            <div class="btn" onClick="inputNum(6)">6</div>
            <div class="btn" onClick="inputOper('+')">+</div>
        </div>
        <div class="row">
            <div class="btn" onClick="inputNum(1)">1</div>
            <div class="btn" onClick="inputNum(2)">2</div>
            <div class="btn" onClick="inputNum(3)">3</div>
            <div class="btn" onClick="inputOper('-')">-</div>
        </div>
        <div class="row">
            <div class="btn btn-lg" onClick="inputNum(0)">0</div>
            <div class="btn" onClick="inputOper('/')">/</div>
            <div class="btn" onClick="inputOper('*')">*</div>
        </div>
    </body>
</html>
```

<br />

### 계산기 로직 구현하기

글로벌 변수를 만들어서 ture일 경우에는 = 이 눌렸다고 설정해준다 <br /> \*원래는 null sefety하게 프로그램을 짜는 것이 좋으나 단순한 로직이기 때문에 이렇게 진행.

```js
let left = null,
    right = null,
    oper = null,
    res = flase;

//left, right, oper가 존재할 경우 input에 적용시켜주는 함수
function save() {
    const inp = document.getElementById("top-inp");
    let value = "";
    if (left === null) return;
    //left가 존재할 경우에는 value에다가 left를 더해주고 띄어쓰기를 추가해준다.
    value += left + " ";
    //결과값을 input에 적어준다
    inp.value = value;

    //연산자가 없을 경우에는 리턴을 해주고 value에 oper를 더해주고 띄어쓰기를 해준다.
    if (oper === null) return;
    value += oper + " ";
    inp.value = value;

    if (right === null) return;
    value += right + " ";
    inp.value = value;

    //만약에 res가 true라면 = 을 눌렀다는 뜻이고
    //= 을 눌렀으니까 value에다가 =을 붙여준다.
    if (res) {
        //결과값을 담아줄 res를 선언하고
        let res = "";
        //= 뒤에다가 연산결과를 만들어 붙여준다
        //switch로 oper를 돌려주고
        //case로 +일 경우에는 left 와 right를 더하는 형식으로 로직을 구성한다.
        switch (oper) {
            case "+":
                res = parseInt(left) + parseInt(right);
                break;
            case "-":
                res = parseInt(left) - parseInt(right);
                break;
            case "*":
                res = parseInt(left) * parseInt(right);
                break;
            case "/":
                res = parseInt(left) / parseInt(right);
                break;
        }
        value += "=" + res;
        inp.value = value;
    }
}
```

<br />

input버튼을 클릭했을 때 이벤트를 받기 위해서 숫자를 넣는 함수를 만들어준다.

```js
function inputNum(num) {
    //oper가 null이면 즉, 연산자가 null일 경우에는 왼쪽이 아직 다 끝나지 않았다면
    if (oper === null) {
        //그리고 left가 null일 경우 즉, 한번도 input이 되지 않았을 경우
        if (left === null) {
            //left가 null일 경우에는 num을 문자로 바꿔서 넣어준다.
            left = `${num}`;
            //null이 아닐 경우에는 left에 문자열을 더해준다.
        } else {
            //0이 여러번 올 수 없기 때문에 가드를 해준다.
            //num도 0이고 left도 0일 때는 아무것도 더하지 않고 그냥 넘어간다.
            if (num === 0 && parseInt(left) === 0) return;
            left += `${num}`;
        }
        console.log("flag1", left);
    }
    //oper가 null이 아닐 경우, number가 들어왔으면 right에다가 적어준다.
    else {
        //연산자가 입력되지 않았는데 숫자가 들어왔다면 계산기 오른쪽에 텍스트를 적어주고
        //right가 null 경우 즉, 입력이 된 적이 없으면 바로 right에 넣어주고
        if (right === null) {
            right = `${num}`;
        }
        //그게 아니면 더해준다.
        else {
            //마찬가지로 둘 다 0 일 경우 return으로 아래의 명령문을 실행하지 않고 패스
            if (num === 0 && parseInt(right) === 0) return;
            right += `${num}`;
        }
    }
    //input을 한다음에 바로 save를 호출해서 버튼을 클릭하면 바로바로 input에
    //적절하게 대응될 수 있게 해준다.
    save();
}
```

<br />

연산자들을 클릭 했을 때 작동하도록 로직 구현

```js
.
.
function inputOper(op) {
  //연산자 같은 경우에는 left가 null이고 연산자가 -인 경우에는 input이 안되도록해준다.
  if (left === null && op === "-") {
    //left가 null일 경우 음수로 사용하겠다.
    left = "-"
    save()
    return;
  }
  //만약에 left가 null은 아닌데 그냥 -만 있다면 연산자를 쓸 수 없게
  if(left === "-" && op === "-") {
    save()
    return;
  }
  //만약에 연산자가 -인데 기존의 값이 존재한고 right가 null이라면
  if(op === "-" && oper !== null && right === null) {
    //-를 넣어주고 바로 save 해준다.
    right = "-"
    save()
    return;
  }
  //그게 아니라면 아래 명령문을 실행한다.
  //더해지는게 아니라 = 이기 때문에 언제든지 대체될 수 있다.
  oper = op;
  save()
}
```

<br>

= 으로 바로 연산되도록 로직 구현

```js
.
.
function inputEqu() {
  //만약 기존에 이미 한번 = 으로 계산이 되었고
  if(res) {
    //계산된 상태에서 = 를 한번 더 눌렀다면 left에 resValue을 넣고
    left = resValue
    //초기화를 한 번 해준다.
    right = null
    resValue = null
    oper = null
    res = false
    //만약에 res가 기존에 false 였다면 ture로만 바꿔준다.
  } else {
      res = true
  }
  save()
}
```

<br />

inputEqu()보강하기

-left, right, oper가 없다면 res에 변화가 가지않도록 블로킹하여 보강하기 <br /> -계산을 할 때 = 을 눌러야만 결과값을 나타내록 보강하기

```js
function inputEqu() {
    //left가 null이거나 right가 null이거나 oper가 null일 때
    //return을 통해서 res가 true가 되는 경우를 막는다.
    if (left === null || right === null || !oper) return;

    if (res) {
        left = resValue;
        right = null;
        resValue = null;
        oper = null;
        res = false;
    } else {
        res = true;
    }
    save();
}
```
