var cash = 1000;
var bet;
var icash = cash;
setNumber("cash", cash);
setText("cashLabel", "Cash:");
setText("betLabel", "Bet:");
setNumber("bet", 0);
onEvent("startButton", "click", function() {
  setScreen("gameScreen");
});

function deal() {
  setNumber("card1", randomNumber(4, 11));
  setNumber("card2", randomNumber(1, 11));
  setNumber("card3", randomNumber(4, 11));
  setNumber("card4", randomNumber(1, 11));
  setNumber("card5", randomNumber(1, 11));
  setNumber("card6", randomNumber(1, 11));
  if (getNumber("card1")==11 & getNumber("card2")==11) {
    setNumber("card1", randomNumber(1, 10));
  }
if (getNumber("card3")==11 & getNumber("card4")==11) {
  setNumber("card3", randomNumber(1, 10));
}
}
function push() {
  setText("result", "Push!");
  cash = ((cash + (bet*2))-bet);
  setText("cash", cash);
  hideElement("back");
}
function lose() {
  cash = cash;
setText("cash", cash);
  hideElement("back");
}
function win() {
  cash = cash + (bet * 2);
  setText("cash", cash);
  hideElement("back");
}
function reset() {
  setNumber("card1", "");
setNumber("card2", "");
setNumber("card3", "");
setNumber("card4", "");
  setNumber("card5", "");
  setNumber("card6", "");
  setText("result", "");
  hideElement("checkButton");
  hideElement("hitButton");
  showElement("betButton");
  hideElement("dealButton");
  showElement("back");
  setText("bet", "");
  hideElement("invalidLabel");
}
function resetButton() {
  showElement("dealButton");
  hideElement("checkButton");
  hideElement("hitButton");
}
onEvent("betButton", "click", function() {
  var pbet = prompt("Place your bet (1-1000):");
  setNumber("bet", pbet);
  bet = pbet;
  cash = cash-bet;
  setNumber("cash", cash);
  hideElement("betButton");
  showElement("hitButton");
  showElement("checkButton");
  deal();
  if (bet>icash) {
    reset();
    showElement("invalidLabel");
    setText("invalidLabel", "Invalid Amount");
    hideElement("checkButton");
    hideElement("betButton");
    hideElement("hitButton");
    setPosition("dealButton", 120, 298);
    resetButton();
    setNumber("cash", 1000);
    cash = 1000;
    onEvent("dealButton", "click", function() {
      setPosition("dealButton", 120, 205);
    });
  }
  if (cash<0) {
    reset();
    setText("result", "Backrupt!");
  }
});
onEvent("checkButton", "click", function() {
  hideElement("back");
  check();
  resetButton();
});
onEvent("dealButton", "click", function() {
  reset();
  hideElement("card5");
  hideElement("card6");
});
onEvent("hitButton", "click", function() {
  var card1 = getNumber("card1");
var card2 = getNumber("card2");
var card3 = getNumber("card3");
var card4 = getNumber("card4");
var card5 = getNumber("card5");
var card6 = getNumber("card6");
var dscore = card3+card4;
var d2score = ((card3+card4)+card6);
var p2score = ((card1+card2)+card5);
showElement("card5");
  hideElement("hitButton");
  if (p2score > 21) {
    setText("result", "Bust!");
    hideElement("back");
    lose();
    resetButton();
  } else {
    if (dscore <= 16) {
      showElement("card6");
      if (d2score>21) {
        win();
setText("result", "Dealer Bust!");
      } else {
        if (p2score==d2score) {
          push();
        } else {
          if (p2score>d2score) {
            setText("result", "Win!");
            win();
          } else {
            if (p2score<d2score) {
              setText("result", "Lose!");
              lose();
            } else if ((p2score<d2score)) {
              lose();
              setText("result", "Lose!");
            }
          }
        }
      }
    } else {
      card6 = 0;
      if (p2score==dscore) {
        push();
      } else if ((p2score>dscore)) {
        setText("result", "Win!");
        win();
      } else {
        lose();
        setText("result", "Lose!");
      }
    }
  }
  resetButton();
});
function check() {
  var card1 = getNumber("card1");
var card2 = getNumber("card2");
var card3 = getNumber("card3");
var card4 = getNumber("card4");
var card6 = getNumber("card6");
  var pscore = card1+card2;
  var dscore = card3+card4;
  var d2score = ((card3+card4)+card6);
  setNumber("card5", 0);
  if (dscore <= 16) {
      showElement("card6");
      if (d2score >= 22) {
        win();
        setText("result", "Dealer Bust!");
      } else if ((pscore==d2score)) {
        push();
      } else if ((pscore<d2score)) {
        setText("result", "Lose!");
        lose();
      } else if ((pscore>d2score)) {
        setText("result", "Win!");
        win();
      }
  } else {
    if (pscore==dscore) {
        setText("result", "Push!");
        push();
      } else {
      if (pscore==21) {
      setText("result", "Blackjack!");
      win();
      } else {
        card6 = 0;
        if (pscore>dscore) {
            setText("result", "Win!");
            win();
          } else {
          setText("result", "Lose!");
          lose();
        }
      }
    }
  }
  resetButton();
  }
