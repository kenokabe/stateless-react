# stateless-react
Stateless React with a proper FRP system

# Installl & Test

Prepaire some project folder

```
$ git clone https://github.com/kenokabe/stateless-react.git

$ cd stateless-react

$ npm install

$ npm test
```

equivalent to

```
$ ./node_modules/.bin/babel ./index.jsx -o ./index0.js; ./node_modules/.bin/webpack ./index0.js ./index.js;

```
Then Open

`index.html` with WebBrowser


### index.jsx

```js
(() => {
  'use strict';

  var React = require('react');
  var ReactDOM = require('react-dom');
  var Immutable = require('immutable');
  var __ = require('timeengine');

  //================================
  var HelloComponent = () => (<div>HelloComponent!!</div>);

  var HelloChildrenComponent = () => (
  <div>
      Hello
      <div>child</div>
      <div>child</div>
      <div>child</div>
  </div>
  );

  var ChildComponent = () => (
  <div>child</div>
  );

  var HelloChildrenComponent2 = () => (
  <div>
      Hello
      {ChildComponent()}
      {ChildComponent()}
      {ChildComponent()}
  </div>
  );

  var ChildrenComponent = () => {

    var elArray = [ChildComponent(),
      ChildComponent(),
      ChildComponent()];

    var el = (<div>{elArray}</div>);

    return el;
  };

  var HelloChildrenComponent3 = () => (
  <div>
      Hello
      {ChildrenComponent()}
  </div>
  );



  var ChildNumberComponent = (number) => (
  <div>child{number}</div>
  );

  var ChildrenNumberComponent3 = () => {

    var el = (
    <div>
        {ChildNumberComponent(0)}
        {ChildNumberComponent(1)}
        {ChildNumberComponent(2)}
    </div>
    );
    return el;

  };

  var ChildrenNumberComponent5 = () => {

    var el = (
    <div>
        {[ChildNumberComponent(0),
      ChildNumberComponent(1),
      ChildNumberComponent(2),
      ChildNumberComponent(3),
      ChildNumberComponent(4)]}
    </div>
    );
    return el;

  };

  var ChildrenNumberComponent10 = () => {

    var elArray
    = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9].map(ChildNumberComponent);
    //number array transforms to element array
    var el = (<div>{elArray}</div>);
    return el;

  };

  var ChildrenNumberComponent100 = () => {

    var elArray
    = Immutable.Range(0, 100).toArray().map(ChildNumberComponent);

    var el = (<div>{elArray}</div>);
    return el;

  };

  //########## FRP ############################

  //***React state with life cycle is stateless sequence*****
  var seqComponent = (__seq) => {

    class SeqComponent extends React.Component {
      constructor() {
        super();
        this.state = {
          seq: __seq.t
        };
        var timeseq = __seq
          .tMap((val) => {
            this.setState({
              seq: val
            });
          });
      }
      render() {
        return (<span>{this.state.seq}</span>);
      };
    }

    return (<SeqComponent/>);
  };
  //**************************************

  var TimerComponent = () => {

    var naturalSeq = Immutable.Range();
    //infinite natural numbers sequence [0...Infinite]

    var __count = __
      .intervalSeq(1000) //1000msec time resolution
      // `seqMap` is to map Immutable-js infinite Sequence
      //                 onto TimeEngine infinite Sequence
      // map natural numbers sequence onto intervalSeq(1000)
      .seqMap(naturalSeq)
      .tMap((count) => {
        __.t = __.log(count);
        return count;
      });
      // as a result, this works as an incremental counter

    var el = (<div>Timer : {seqComponent(__count)}</div>);

    return el;
  };

  //-------Physics-------------------------------

  //MKS system of units
  var V0 = 85.0; // m/s
  var DEG = 40; //degree
  var THETA = DEG / 180 * Math.PI; //radian
  var G = 9.8; //gravity const

  //10msec time resolution
  //t seconds elapsed since t0
  var t = __.intervalSeq(10).tMap((tt, t0) => (tt - t0) / 1000);

  var x = t.tMap((t) => V0 * Math.cos(THETA) * t);

  var y = t.tMap((t) => V0 * Math.sin(THETA) * t - 1 / 2 * G * Math.pow(t, 2));

  //==============================================================
  var Drawscale = 2; //2 dot = 1 meter

  var PhysicsComponent = () => {

    var __seqElement = __([x, y]) //atomic update
      .tMap(([x, y]) => (
      <div>
        <svg height = "100%"  width = "100%">
            <circle r="3" fill="red"
        cx = {50 + x * Drawscale} cy = {500 - y * Drawscale}/>
        </svg>
      </div>));

    var el = (<div>{seqComponent(__seqElement)}</div>);

    return el;
  };



  //====================================

  var HelloElement = (
  <div>
      <p>HelloElement!!</p>
      =====================
      {HelloComponent()}
      =====================
      {HelloChildrenComponent()}
      =====================
      {HelloChildrenComponent2()}
      =====================
      {HelloChildrenComponent3()}
      =====================
      {ChildrenNumberComponent3()}
      =====================
      {ChildrenNumberComponent5()}
      =====================
      {ChildrenNumberComponent10()}
      =====================
      {ChildrenNumberComponent100()}
      =====================
      {TimerComponent()}
      =====================
      <p>Scroll Down for Physics FRP DEMO</p>
      {PhysicsComponent()}
      =====================
  </div>
  );

  var mount = ReactDOM.render(HelloElement, document.getElementById('container'));

//===================================
})();
```


### result


```js

HelloElement!!

=====================
HelloComponent!!
=====================
Hello
child
child
child
=====================
Hello
child
child
child
=====================
Hello
child
child
child
=====================
child0
child1
child2
=====================
child0
child1
child2
child3
child4
=====================
child0
child1
child2
child3
child4
child5
child6
child7
child8
child9
=====================
child0
child1
child2
child3
child4
child5
child6
child7
child8
child9
child10
child11
child12
child13
child14
child15
child16
child17
child18
child19
child20
child21
child22
child23
child24
child25
child26
child27
child28
child29
child30
child31
child32
child33
child34
child35
child36
child37
child38
child39
child40
child41
child42
child43
child44
child45
child46
child47
child48
child49
child50
child51
child52
child53
child54
child55
child56
child57
child58
child59
child60
child61
child62
child63
child64
child65
child66
child67
child68
child69
child70
child71
child72
child73
child74
child75
child76
child77
child78
child79
child80
child81
child82
child83
child84
child85
child86
child87
child88
child89
child90
child91
child92
child93
child94
child95
child96
child97
child98
child99
=====================
Timer : 53
=====================
Scroll Down for Physics FRP DEMO
```
