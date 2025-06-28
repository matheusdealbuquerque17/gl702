import React, { useState, useEffect } from 'react';
import { Home, MapPin, Utensils, Book, Hammer, Store, Shirt, Footprints, Cake, Flower, Gamepad, PawPrint, ShoppingBag, User } from 'lucide-react'; // Added User icon for profile

// Main App Component
const App = () => {
  const [isLoggedIn, setIsLoggedIn] = useState(false);
  const [currentPage, setCurrentPage] = useState('home'); // 'home', 'map', 'businessTypes', 'profile'
  const [points, setPoints] = useState(1250); // Example points
  const pointsToRealRatio = 0.05; // 1 point = R$ 0.05

  const handleLogin = (username, password) => {
    // Simple simulated login. In a real app, you'd validate credentials.
    if (username && password) {
      setIsLoggedIn(true);
      setCurrentPage('home'); // Navigate to home after login
    } else {
      // Replaced alert with a simple console log for demonstration,
      // as alerts are not ideal in an iframe and custom modals are preferred.
      console.log('Por favor, insira usuário e senha.');
      // In a real app, you'd show a user-friendly message in the UI.
    }
  };

  const renderContent = () => {
    if (!isLoggedIn) {
      return <LoginScreen onLogin={handleLogin} />;
    } else {
      return (
        <div className="flex flex-col min-h-screen bg-gray-100">
          <PointsBar points={points} ratio={pointsToRealRatio} />

          <div className="flex-grow p-4 md:p-8">
            {currentPage === 'home' && (
              <HomeScreen setCurrentPage={setCurrentPage} />
            )}
            {currentPage === 'map' && (
              <InteractiveMapView /> // Using the new interactive map view
            )}
            {currentPage === 'businessTypes' && (
              <BusinessTypesView />
            )}
            {currentPage === 'profile' && (
              <ProfileScreen />
            )}
          </div>

          {/* Simple Navigation Bar at the bottom for illustration */}
          <div className="fixed bottom-0 left-0 right-0 bg-white shadow-lg p-4 flex justify-around items-center border-t border-gray-200">
            <button
              onClick={() => setCurrentPage('home')}
              className={`flex flex-col items-center p-2 rounded-lg ${currentPage === 'home' ? 'text-blue-600 bg-blue-50' : 'text-gray-600 hover:bg-gray-100'}`}
            >
              <Home size={24} />
              <span className="text-xs mt-1">Início</span>
            </button>
            <button
              onClick={() => setCurrentPage('map')}
              className={`flex flex-col items-center p-2 rounded-lg ${currentPage === 'map' ? 'text-blue-600 bg-blue-50' : 'text-gray-600 hover:bg-gray-100'}`}
            >
              <MapPin size={24} />
              <span className="text-xs mt-1">Mapa</span>
            </button>
            <button
              onClick={() => setCurrentPage('businessTypes')}
              className={`flex flex-col items-center p-2 rounded-lg ${currentPage === 'businessTypes' ? 'text-blue-600 bg-blue-50' : 'text-gray-600 hover:bg-gray-100'}`}
            >
              <Store size={24} />
              <span className="text-xs mt-1">Negócios</span>
            </button>
            <button
              onClick={() => setCurrentPage('profile')}
              className={`flex flex-col items-center p-2 rounded-lg ${currentPage === 'profile' ? 'text-blue-600 bg-blue-50' : 'text-gray-600 hover:bg-gray-100'}`}
            >
              <User size={24} />
              <span className="text-xs mt-1">Perfil</span>
            </button>
          </div>
        </div>
      );
    }
  };

  return (
    <div className="font-sans antialiased text-gray-800">
      <script src="https://cdn.tailwindcss.com"></script>
      {/* Tailwind CSS configuration */}
      <style>{`
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');
        body {
          font-family: 'Inter', sans-serif;
        }
      `}</style>
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      {renderContent()}
    </div>
  );
};

// Login Screen Component
const LoginScreen = ({ onLogin }) => {
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
    onLogin(username, password);
  };

  return (
    <div className="min-h-screen flex items-center justify-center bg-gradient-to-br from-blue-500 to-indigo-600 p-4">
      <div className="bg-white p-8 rounded-xl shadow-2xl w-full max-w-md transform transition-all duration-300 hover:scale-105">
        <h2 className="text-4xl font-extrabold text-center text-gray-900 mb-8">Bem-vindo(a)!</h2>
        <p className="text-center text-gray-600 mb-8">Faça login para gerenciar seus pontos de fidelidade.</p>
        <form onSubmit={handleSubmit} className="space-y-6">
          <div>
            <label htmlFor="username" className="block text-sm font-medium text-gray-700 mb-2">
              Usuário ou E-mail
            </label>
            <input
              type="text"
              id="username"
              className="mt-1 block w-full px-4 py-3 border border-gray-300 rounded-lg shadow-sm focus:ring-blue-500 focus:border-blue-500 transition duration-150 ease-in-out"
              placeholder="seu.email@exemplo.com"
              value={username}
              onChange={(e) => setUsername(e.target.value)}
              required
            />
          </div>
          <div>
            <label htmlFor="password" className="block text-sm font-medium text-gray-700 mb-2">
              Senha
            </label>
            <input
              type="password"
              id="password"
              className="mt-1 block w-full px-4 py-3 border border-gray-300 rounded-lg shadow-sm focus:ring-blue-500 focus:border-blue-500 transition duration-150 ease-in-out"
              placeholder="••••••••"
              value={password}
              onChange={(e) => setPassword(e.target.value)}
              required
            />
          </div>
          <button
            type="submit"
            className="w-full flex justify-center py-3 px-4 border border-transparent rounded-lg shadow-lg text-lg font-semibold text-white bg-blue-600 hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500 transition duration-200 ease-in-out transform hover:-translate-y-1"
          >
            Entrar
          </button>
        </form>
        <p className="mt-8 text-center text-sm text-gray-500">
          Não tem uma conta? <a href="#" className="font-medium text-blue-600 hover:text-blue-500">Cadastre-se</a>
        </p>
      </div>
    </div>
  );
};

// Points Bar Component
const PointsBar = ({ points, ratio }) => {
  const realValue = (points * ratio).toFixed(2);

  return (
    <div className="w-full bg-gradient-to-r from-blue-600 to-purple-700 text-white p-4 shadow-lg sticky top-0 z-30 rounded-b-xl"> {/* Changed z-index to z-30 */}
      <div className="flex justify-between items-center max-w-7xl mx-auto">
        <div className="flex flex-col">
          <span className="text-sm opacity-80">Seus Pontos:</span>
          <span className="text-3xl font-bold">{points}</span>
        </div>
        <div className="flex flex-col items-end">
          <span className="text-sm opacity-80">Valor em R$:</span>
          <span className="text-3xl font-bold">R$ {realValue}</span>
        </div>
      </div>
    </div>
  );
};

// Home Screen Component - Now just a container for buttons
const HomeScreen = ({ setCurrentPage }) => {
  return (
    <div className="flex flex-col items-center justify-center p-4 min-h-[calc(100vh-160px)]"> {/* Adjusted min-height for points bar and bottom nav */}
      <h1 className="text-3xl md:text-4xl font-bold text-gray-900 mb-8 text-center w-full">Navegue pelo App</h1> {/* Added w-full for better centering */}

      <div className="grid grid-cols-1 md:grid-cols-2 gap-6 w-full max-w-4xl">
        <button
          onClick={() => setCurrentPage('map')}
          className="bg-green-500 text-white px-6 py-4 rounded-lg shadow-lg hover:bg-green-600 transition-all duration-200 ease-in-out transform hover:-translate-y-1 flex items-center justify-center text-xl font-semibold"
        >
          <MapPin size={28} className="mr-3" /> Ver Lojas no Mapa
        </button>

        <button
          onClick={() => setCurrentPage('businessTypes')}
          className="bg-purple-500 text-white px-6 py-4 rounded-lg shadow-lg hover:bg-purple-600 transition-all duration-200 ease-in-out transform hover:-translate-y-1 flex items-center justify-center text-xl font-semibold"
        >
          <Store size={28} className="mr-3" /> Tipos de Negócios
        </button>
      </div>
    </div>
  );
};

// New Interactive Map View Component
const InteractiveMapView = () => {
  const [selectedCategory, setSelectedCategory] = useState('all');

  // Define business data with ficticious positions (x, y percentages)
  const businessCategoriesMap = [
    {
      name: 'Restaurantes',
      key: 'restaurants',
      icon: <Utensils size={18} />,
      businesses: [
        { name: 'Sabor da Vila', x: '15%', y: '20%' },
        { name: 'Cantinho do Prato', x: '40%', y: '10%' },
        { name: 'Dona Maria Marmitas', x: '70%', y: '30%' },
      ],
    },
    {
      name: 'Papelarias',
      key: 'stationery',
      icon: <Book size={18} />,
      businesses: [
        { name: 'Papel & Cia', x: '25%', y: '50%' },
        { name: 'Mundo das Cores', x: '55%', y: '40%' },
        { name: 'Ponto do Papel', x: '80%', y: '60%' },
      ],
    },
    {
      name: 'Lojas de Materiais de Construção',
      key: 'construction',
      icon: <Hammer size={18} />,
      businesses: [
        { name: 'Casa Forte Materiais', x: '10%', y: '80%' },
        { name: 'Constrói Fácil', x: '45%', y: '75%' },
        { name: 'Depósito do Zé', x: '65%', y: '90%' },
      ],
    },
    {
      name: 'Loja de Roupas',
      key: 'clothes',
      icon: <Shirt size={18} />,
      businesses: [
        { name: 'Moda da Vila', x: '5%', y: '5%' },
        { name: 'Estilo & Cia', x: '20%', y: '35%' },
        { name: 'Boutique Dona Rosa', x: '50%', y: '25%' },
      ],
    },
    {
      name: 'Loja de Calçados',
      key: 'shoes',
      icon: <Footprints size={18} />,
      businesses: [
        { name: 'Pé na Rua', x: '35%', y: '55%' },
        { name: 'Passo Certo Calçados', x: '60%', y: '45%' },
        { name: 'Sapataria São Jorge', x: '85%', y: '50%' },
      ],
    },
    {
      name: 'Padaria',
      key: 'bakery',
      icon: <Cake size={18} />,
      businesses: [
        { name: 'Pão Nosso', x: '10%', y: '65%' },
        { name: 'Sabor do Trigo', x: '30%', y: '85%' },
        { name: 'Padoca do Bairro', x: '70%', y: '70%' },
      ],
    },
    {
      name: 'Doceria/Confeitaria',
      key: 'confectionery',
      icon: <Cake size={18} />,
      businesses: [
        { name: 'Doce Encanto', x: '20%', y: '5%' },
        { name: 'Delícias da Ana', x: '45%', y: '20%' },
        { name: 'Casa do Açúcar', x: '75%', y: '10%' },
      ],
    },
    {
      name: 'Floricultura',
      key: 'florist',
      icon: <Flower size={18} />,
      businesses: [
        { name: 'Flor & Vida', x: '5%', y: '90%' },
        { name: 'Encanto das Flores', x: '30%', y: '95%' },
        { name: 'Jardim da Vila', x: '60%', y: '80%' },
      ],
    },
    {
      name: 'Loja de Brinquedos',
      key: 'toys',
      icon: <Gamepad size={18} />,
      businesses: [
        { name: 'Mundo da Criança', x: '10%', y: '40%' },
        { name: 'BrincaBrinca', x: '50%', y: '60%' },
        { name: 'Casa dos Sonhos', x: '90%', y: '20%' },
      ],
    },
    {
      name: 'Pet Shop',
      key: 'petshop',
      icon: <PawPrint size={18} />,
      businesses: [
        { name: 'Bicho Feliz', x: '25%', y: '70%' },
        { name: 'Pet da Vila', x: '70%', y: '55%' },
        { name: 'Mundo Animal Pet', x: '95%', y: '75%' },
      ],
    },
    {
      name: 'Loja de Utilidades (1,99, variedades)',
      key: 'utilities',
      icon: <ShoppingBag size={18} />,
      businesses: [
        { name: 'Útil & Barato', x: '40%', y: '5%' },
        { name: 'Casa Pronta', x: '80%', y: '85%' },
        { name: 'Tudo Tem', x: '90%', y: '35%' },
      ],
    },
  ];

  // Flatten all businesses for easy filtering, adding category key and icon to each business
  const allBusinessesFlattened = businessCategoriesMap.flatMap(category =>
    category.businesses.map(business => ({
      ...business,
      category: category.key,
      icon: category.icon,
    }))
  );

  const filteredBusinesses = selectedCategory === 'all'
    ? allBusinessesFlattened
    : allBusinessesFlattened.filter(b => b.category === selectedCategory);

  return (
    <div className="flex flex-col items-center p-4">
      <h1 className="text-3xl md:text-4xl font-bold text-gray-900 mb-6 text-center">Lojas Parceiras no Mapa</h1>
      <p className="text-lg text-gray-700 mb-8 text-center max-w-2xl">
        Explore a localização das nossas lojas parceiras. Use os filtros abaixo para segmentar por tipo de negócio.
      </p>

      <div className="flex flex-wrap justify-center gap-3 mb-8 w-full max-w-4xl">
        <button
          onClick={() => setSelectedCategory('all')}
          className={`px-5 py-2 rounded-full text-sm font-medium transition-all duration-200 ${
            selectedCategory === 'all'
              ? 'bg-blue-600 text-white shadow-md'
              : 'bg-gray-200 text-gray-700 hover:bg-gray-300'
          }`}
        >
          Todas
        </button>
        {businessCategoriesMap.map((category) => (
          <button
            key={category.key}
            onClick={() => setSelectedCategory(category.key)}
            className={`px-5 py-2 rounded-full text-sm font-medium transition-all duration-200 flex items-center gap-2 ${
              selectedCategory === category.key
                ? 'bg-blue-600 text-white shadow-md'
                : 'bg-gray-200 text-gray-700 hover:bg-gray-300'
            }`}
          >
            {category.icon} {category.name}
          </button>
        ))}
      </div>

      <div className="w-full max-w-3xl h-[500px] bg-white rounded-xl shadow-lg overflow-hidden border border-gray-200 relative">
        {/* Simulated map background with roads, parks, and squares */}
        <div className="absolute inset-0 bg-gray-100">
          {/* Main roads */}
          <div className="absolute bg-gray-400 h-4 w-full top-1/4 transform -translate-y-1/2"></div>
          <div className="absolute bg-gray-400 h-4 w-full top-3/4 transform -translate-y-1/2"></div>
          <div className="absolute bg-gray-400 w-4 h-full left-1/4 transform -translate-x-1/2"></div>
          <div className="absolute bg-gray-400 w-4 h-full left-3/4 transform -translate-x-1/2"></div>

          {/* Smaller streets/blocks */}
          <div className="absolute bg-gray-300 h-2 w-1/3 top-[10%] left-[5%]"></div>
          <div className="absolute bg-gray-300 h-2 w-1/4 top-[30%] right-[10%]"></div>
          <div className="absolute bg-gray-300 h-2 w-1/5 bottom-[15%] left-[20%]"></div>
          <div className="absolute bg-gray-300 w-2 h-1/4 top-[5%] left-[60%]"></div>
          <div className="absolute bg-gray-300 w-2 h-1/5 bottom-[5%] right-[5%]"></div>

          {/* Parks (green areas) */}
          <div className="absolute bg-green-300 rounded-full w-24 h-24 top-[5%] left-[70%] opacity-70"></div>
          <div className="absolute bg-green-300 rounded-lg w-32 h-20 bottom-[10%] left-[5%] opacity-70"></div>

          {/* Squares (light brown/tan areas) */}
          <div className="absolute bg-amber-200 w-20 h-20 top-[45%] left-[45%] rounded-lg opacity-80"></div>
          <div className="absolute bg-amber-200 w-16 h-16 bottom-[30%] right-[20%] rounded-full opacity-80"></div>

          {/* Overlaid grid for visual separation */}
          <div className="absolute inset-0 grid grid-cols-5 grid-rows-5 border border-gray-200">
            {Array.from({ length: 25 }).map((_, i) => (
              <div key={i} className="border border-gray-200 opacity-50"></div>
            ))}
          </div>
        </div>

        {filteredBusinesses.map((business, index) => (
          <div
            key={index}
            style={{ left: business.x, top: business.y }}
            className="absolute p-2 bg-white rounded-lg shadow-md border border-gray-300 text-sm font-medium text-gray-800 flex items-center transform -translate-x-1/2 -translate-y-1/2 cursor-pointer hover:scale-105 transition-transform duration-150 z-20"
            title={business.name} // Tooltip on hover
          >
            {business.icon}
            <span className="ml-1 whitespace-nowrap">{business.name}</span>
          </div>
        ))}
      </div>
      <p className="mt-4 text-center text-sm text-gray-600">
        As localizações são meramente ilustrativas para demonstração.
      </p>
    </div>
  );
};

// Business Types View Component (unchanged)
const BusinessTypesView = () => {
  const businessCategories = [
    {
      name: 'Restaurantes',
      icon: <Utensils size={24} className="text-orange-500" />,
      businesses: [
        'Sabor da Vila',
        'Cantinho do Prato',
        'Dona Maria Marmitas',
      ],
    },
    {
      name: 'Papelarias',
      icon: <Book size={24} className="text-blue-500" />,
      businesses: [
        'Papel & Cia',
        'Mundo das Cores',
        'Ponto do Papel',
      ],
    },
    {
      name: 'Lojas de Materiais de Construção',
      icon: <Hammer size={24} className="text-red-500" />,
      businesses: [
        'Casa Forte Materiais',
        'Constrói Fácil',
        'Depósito do Zé',
      ],
    },
    {
      name: 'Loja de Roupas',
      icon: <Shirt size={24} className="text-pink-500" />,
      businesses: [
        'Moda da Vila',
        'Estilo & Cia',
        'Boutique Dona Rosa',
      ],
    },
    {
      name: 'Loja de Calçados',
      icon: <Footprints size={24} className="text-indigo-500" />,
      businesses: [
        'Pé na Rua',
        'Passo Certo Calçados',
        'Sapataria São Jorge',
      ],
    },
    {
      name: 'Padaria',
      icon: <Cake size={24} className="text-yellow-600" />, // Using Cake for bread/bakery
      businesses: [
        'Pão Nosso',
        'Sabor do Trigo',
        'Padoca do Bairro',
      ],
    },
    {
      name: 'Doceria/Confeitaria',
      icon: <Cake size={24} className="text-purple-500" />,
      businesses: [
        'Doce Encanto',
        'Delícias da Ana',
        'Casa do Açúcar',
      ],
    },
    {
      name: 'Floricultura',
      icon: <Flower size={24} className="text-green-500" />,
      businesses: [
        'Flor & Vida',
        'Encanto das Flores',
        'Jardim da Vila',
      ],
    },
    {
      name: 'Loja de Brinquedos',
      icon: <Gamepad size={24} className="text-cyan-500" />, // Changed from Toy to Gamepad
      businesses: [
        'Mundo da Criança',
        'BrincaBrinca',
        'Casa dos Sonhos',
      ],
    },
    {
      name: 'Pet Shop',
      icon: <PawPrint size={24} className="text-brown-500" />, // Custom color for brown
      businesses: [
        'Bicho Feliz',
        'Pet da Vila',
        'Mundo Animal Pet',
      ],
    },
    {
      name: 'Loja de Utilidades (1,99, variedades)',
      icon: <ShoppingBag size={24} className="text-gray-600" />,
      businesses: [
        'Útil & Barato',
        'Casa Pronta',
        'Tudo Tem',
      ],
    },
  ];

  return (
    <div className="flex flex-col items-center p-4">
      <h1 className="text-3xl md:text-4xl font-bold text-gray-900 mb-6 text-center">Tipos de Negócios Parceiros</h1>
      <p className="text-lg text-gray-700 mb-8 text-center max-w-2xl">
        Conheça os diferentes tipos de estabelecimentos onde você pode acumular e usar seus pontos.
      </p>

      <div className="w-full max-w-3xl space-y-6">
        {businessCategories.map((category, index) => (
          <div key={index} className="bg-white p-6 rounded-xl shadow-md border border-gray-200">
            <div className="flex items-center mb-4">
              {category.icon}
              <h2 className="text-2xl font-semibold ml-3 text-gray-800">{category.name}</h2>
            </div>
            <ul className="list-disc list-inside space-y-2 text-gray-700">
              {category.businesses.map((business, bIndex) => (
                <li key={bIndex} className="flex items-center">
                  <span className="mr-2 text-green-500">•</span> {business}
                </li>
              ))}
            </ul>
          </div>
        ))}
      </div>
    </div>
  );
};

// New Profile Screen Component
const ProfileScreen = () => {
  // Placeholder user data
  const userData = {
    name: 'Nome do Usuário',
    email: 'usuario.exemplo@email.com',
    joinDate: '15 de Março de 2023',
    cpf: '123.456.789-00',
    // You could add more fields here like address, phone, etc.
  };

  return (
    <div className="flex flex-col items-center p-4">
      <h1 className="text-3xl md:text-4xl font-bold text-gray-900 mb-6 text-center">Meu Perfil</h1>
      <p className="text-lg text-gray-700 mb-8 text-center max-w-2xl">
        Aqui estão suas informações básicas de cadastro.
      </p>

      <div className="w-full max-w-md bg-white p-8 rounded-xl shadow-lg border border-gray-200 space-y-6">
        <div>
          <label className="block text-sm font-medium text-gray-600 mb-1">Nome Completo:</label>
          <p className="text-gray-800 text-lg font-semibold">{userData.name}</p>
        </div>
        <div>
          <label className="block text-sm font-medium text-gray-600 mb-1">E-mail:</label>
          <p className="text-gray-800 text-lg">{userData.email}</p>
        </div>
        <div>
          <label className="block text-sm font-medium text-gray-600 mb-1">Membro desde:</label>
          <p className="text-gray-800 text-lg">{userData.joinDate}</p>
        </div>
        <div>
          <label className="block text-sm font-medium text-gray-600 mb-1">CPF:</label>
          <p className="text-gray-800 text-lg">{userData.cpf}</p>
        </div>
        {/* You could add an "Edit Profile" button here */}
        <button className="w-full bg-blue-600 text-white px-6 py-3 rounded-lg shadow-md hover:bg-blue-700 transition-all duration-200 ease-in-out transform hover:-translate-y-1 text-lg font-semibold">
          Editar Perfil
        </button>
      </div>
    </div>
  );
};

export default App;
