# Gastos
Lançamento de gastos
import React, { useState } from 'react';

export default function ControleGastosApp() {
  const [gastos, setGastos] = useState([]);
  const [formData, setFormData] = useState({
    data: new Date().toISOString().split('T')[0],
    valor: '',
    descricao: '',
    formaPagamento: 'PIX',
    categoria: 'Conta Fixa',
    quemLancou: 'João'
  });

  const handleChange = (e) => {
    setFormData({ ...formData, [e.target.name]: e.target.value });
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    if (!formData.valor || !formData.descricao) return;

    const novoGasto = {
      ...formData,
      id: Date.now(),
      valor: parseFloat(formData.valor)
    };

    setGastos([novoGasto, ...gastos]);
    setFormData({ ...formData, valor: '', descricao: '' }); // Limpa os campos de digitação após salvar
  };

  const totalGastos = gastos.reduce((acc, curr) => acc + curr.valor, 0);

  return (
    <div className="min-h-screen bg-gray-100 p-4 font-sans">
      <div className="max-w-md mx-auto bg-white rounded-xl shadow-md overflow-hidden p-6">
        <h1 className="text-2xl font-bold text-center text-green-700 mb-6">Lançamento Rápido</h1>
        
        <form onSubmit={handleSubmit} className="space-y-4">
          
          {/* Valor */}
          <div>
            <label className="block text-sm font-medium text-gray-700">Valor (R$)</label>
            <input 
              type="number" 
              name="valor" 
              step="0.01"
              value={formData.valor} 
              onChange={handleChange} 
              className="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-3 border focus:ring-green-500 focus:border-green-500 text-lg"
              placeholder="0,00"
              required
            />
          </div>

          {/* Descrição */}
          <div>
            <label className="block text-sm font-medium text-gray-700">Descrição</label>
            <input 
              type="text" 
              name="descricao" 
              value={formData.descricao} 
              onChange={handleChange} 
              className="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-3 border focus:ring-green-500 focus:border-green-500"
              placeholder="Ex: Energia, Pão, Gasolina..."
              required
            />
          </div>

          {/* Forma de Pagamento */}
          <div>
            <label className="block text-sm font-medium text-gray-700">Forma de Pagamento</label>
            <select 
              name="formaPagamento" 
              value={formData.formaPagamento} 
              onChange={handleChange} 
              className="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-3 border focus:ring-green-500 focus:border-green-500 bg-white"
            >
              <option value="PIX">PIX</option>
              <option value="Cartão Nubank">Cartão Nubank</option>
              <option value="Cartão Mercado Pago">Cartão Mercado Pago</option>
              <option value="Cartão Porto Seguro">Cartão Porto Seguro</option>
              <option value="Cartão Magalu">Cartão Magalu</option>
              <option value="Débito">Débito</option>
              <option value="Dinheiro">Dinheiro</option>
            </select>
          </div>

          {/* Categoria */}
          <div>
            <label className="block text-sm font-medium text-gray-700">Categoria</label>
            <select 
              name="categoria" 
              value={formData.categoria} 
              onChange={handleChange} 
              className="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-3 border focus:ring-green-500 focus:border-green-500 bg-white"
            >
              <option value="Conta Fixa">Conta Fixa (Água, Luz, Internet...)</option>
              <option value="Alimentação">Alimentação (Mercado, Padaria)</option>
              <option value="Transporte">Transporte (Combustível, Uber)</option>
              <option value="Compra Parcelada">Compra Parcelada</option>
              <option value="Lazer">Lazer</option>
              <option value="Saúde">Saúde / Farmácia</option>
              <option value="Outros">Outros</option>
            </select>
          </div>

          {/* Quem Lançou e Data na mesma linha */}
          <div className="flex space-x-4">
            <div className="w-1/2">
              <label className="block text-sm font-medium text-gray-700">Quem?</label>
              <select 
                name="quemLancou" 
                value={formData.quemLancou} 
                onChange={handleChange} 
                className="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-3 border bg-white"
              >
                <option value="João">João</option>
                <option value="Esposa">Esposa</option>
              </select>
            </div>
            <div className="w-1/2">
              <label className="block text-sm font-medium text-gray-700">Data</label>
              <input 
                type="date" 
                name="data" 
                value={formData.data} 
                onChange={handleChange} 
                className="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-3 border"
              />
            </div>
          </div>

          <button 
            type="submit" 
            className="w-full bg-green-600 text-white font-bold py-3 px-4 rounded-md hover:bg-green-700 transition duration-200"
          >
            Lançar Gasto
          </button>
        </form>
      </div>

      {/* Histórico Recente e Total */}
      <div className="max-w-md mx-auto mt-6 bg-white rounded-xl shadow-md overflow-hidden p-6">
        <div className="flex justify-between items-center mb-4">
          <h2 className="text-xl font-bold text-gray-800">Lançamentos Recentes</h2>
          <span className="text-lg font-bold text-red-600">Total: R$ {totalGastos.toFixed(2)}</span>
        </div>
        
        {gastos.length === 0 ? (
          <p className="text-gray-500 text-center py-4">Nenhum gasto lançado ainda.</p>
        ) : (
          <ul className="divide-y divide-gray-200">
            {gastos.map((gasto) => (
              <li key={gasto.id} className="py-3 flex justify-between items-center">
                <div>
                  <p className="font-semibold text-gray-800">{gasto.descricao}</p>
                  <p className="text-xs text-gray-500">
                    {gasto.formaPagamento} • {gasto.categoria} • {gasto.quemLancou}
                  </p>
                </div>
                <span className="font-bold text-gray-700">R$ {gasto.valor.toFixed(2)}</span>
              </li>
            ))}
          </ul>
        )}
      </div>
    </div>
  );
}

