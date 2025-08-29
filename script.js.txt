const orderForm = document.getElementById("orderForm");
const ordersDiv = document.getElementById("orders");
const historyDiv = document.getElementById("history");

let orders = JSON.parse(localStorage.getItem("orders")) || [];
let history = JSON.parse(localStorage.getItem("history")) || [];

function saveData() {
  localStorage.setItem("orders", JSON.stringify(orders));
  localStorage.setItem("history", JSON.stringify(history));
}

function render() {
  ordersDiv.innerHTML = "";
  historyDiv.innerHTML = "";

  orders.forEach((o, i) => {
    let div = document.createElement("div");
    div.className = "order status-" + o.status;
    div.innerHTML = `
      <b>${o.client}</b> (${o.phone})<br>
      🚲 ${o.problem}<br>
      🛠 ${o.comment}<br>
      Запчасть: ${o.part} (себестоимость ${o.cost}р, продано ${o.price}р)<br>
      Статус: ${o.status}
      <br><button onclick="finishOrder(${i})">Завершить</button>
    `;
    ordersDiv.appendChild(div);
  });

  history.forEach((o) => {
    let div = document.createElement("div");
    div.className = "order";
    div.innerHTML = `
      ✅ <b>${o.client}</b> (${o.phone}) — ${o.problem}<br>
      Запчасть: ${o.part}, себестоимость ${o.cost}р, продано ${o.price}р
    `;
    historyDiv.appendChild(div);
  });
}

orderForm.addEventListener("submit", e => {
  e.preventDefault();
  const order = {
    client: client.value,
    phone: phone.value,
    problem: problem.value,
    comment: comment.value,
    part: part.value,
    cost: cost.value,
    price: price.value,
    status: status.value
  };
  orders.push(order);
  saveData();
  render();
  orderForm.reset();
});

function finishOrder(index) {
  history.push(orders[index]);
  orders.splice(index, 1);
  saveData();
  render();
}

render();
