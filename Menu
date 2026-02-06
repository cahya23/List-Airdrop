import React, { useState, useEffect } from 'react';
import { 
  Plus, 
  Trash2, 
  ExternalLink, 
  CheckCircle2, 
  Circle, 
  Wallet, 
  Calendar, 
  Search,
  Filter,
  Tag,
  AlertCircle,
  Clock,
  ChevronRight
} from 'lucide-react';

const App = () => {
  const [projects, setProjects] = useState([]);
  const [isModalOpen, setIsModalOpen] = useState(false);
  const [searchTerm, setSearchTerm] = useState('');
  const [filterType, setFilterType] = useState('All');
  const [newProject, setNewProject] = useState({
    name: '',
    type: 'Mainnet',
    status: 'Active',
    wallet: '',
    link: '',
    notes: '',
    tasks: []
  });

  // Load data dari local storage
  useEffect(() => {
    const saved = localStorage.getItem('airdrop_data');
    if (saved) {
      setProjects(JSON.parse(saved));
    }
  }, []);

  // Save data ke local storage
  useEffect(() => {
    localStorage.setItem('airdrop_data', JSON.stringify(projects));
  }, [projects]);

  const handleAddProject = () => {
    if (!newProject.name) return;
    setProjects([...projects, { ...newProject, id: Date.now(), createdAt: new Date().toISOString() }]);
    setNewProject({ name: '', type: 'Mainnet', status: 'Active', wallet: '', link: '', notes: '', tasks: [] });
    setIsModalOpen(false);
  };

  const deleteProject = (id) => {
    setProjects(projects.filter(p => p.id !== id));
  };

  const toggleTask = (projectId, taskIndex) => {
    const updatedProjects = projects.map(p => {
      if (p.id === projectId) {
        const newTasks = [...p.tasks];
        newTasks[taskIndex].completed = !newTasks[taskIndex].completed;
        return { ...p, tasks: newTasks };
      }
      return p;
    });
    setProjects(updatedProjects);
  };

  const addTask = (projectId, taskTitle) => {
    if (!taskTitle) return;
    const updatedProjects = projects.map(p => {
      if (p.id === projectId) {
        return { ...p, tasks: [...p.tasks, { title: taskTitle, completed: false }] };
      }
      return p;
    });
    setProjects(updatedProjects);
  };

  const filteredProjects = projects.filter(p => {
    const matchesSearch = p.name.toLowerCase().includes(searchTerm.toLowerCase());
    const matchesFilter = filterType === 'All' || p.type === filterType;
    return matchesSearch && matchesFilter;
  });

  return (
    <div className="min-h-screen bg-slate-50 text-slate-900 font-sans">
      {/* Navbar */}
      <nav className="bg-indigo-600 text-white p-4 sticky top-0 z-10 shadow-lg">
        <div className="max-w-5xl mx-auto flex justify-between items-center">
          <div className="flex items-center gap-2">
            <div className="bg-white p-1.5 rounded-lg text-indigo-600">
              <Clock size={24} />
            </div>
            <h1 className="text-xl font-bold tracking-tight">Airdrop Tracker</h1>
          </div>
          <button 
            onClick={() => setIsModalOpen(true)}
            className="bg-indigo-500 hover:bg-indigo-400 px-4 py-2 rounded-lg flex items-center gap-2 transition-all font-medium text-sm"
          >
            <Plus size={18} /> Proyek Baru
          </button>
        </div>
      </nav>

      <main className="max-w-5xl mx-auto p-4 md:p-6">
        {/* Search & Filter */}
        <div className="flex flex-col md:flex-row gap-4 mb-8">
          <div className="relative flex-1">
            <Search className="absolute left-3 top-1/2 -translate-y-1/2 text-slate-400" size={18} />
            <input 
              type="text" 
              placeholder="Cari nama proyek..." 
              className="w-full pl-10 pr-4 py-2.5 rounded-xl border border-slate-200 focus:ring-2 focus:ring-indigo-500 outline-none transition-all shadow-sm"
              value={searchTerm}
              onChange={(e) => setSearchTerm(e.target.value)}
            />
          </div>
          <div className="flex gap-2 overflow-x-auto pb-2 md:pb-0">
            {['All', 'Mainnet', 'Testnet', 'Potential', 'Completed'].map((type) => (
              <button
                key={type}
                onClick={() => setFilterType(type)}
                className={`px-4 py-2 rounded-xl text-sm font-medium whitespace-nowrap transition-all ${
                  filterType === type 
                  ? 'bg-indigo-600 text-white shadow-md shadow-indigo-200' 
                  : 'bg-white text-slate-600 border border-slate-200 hover:border-indigo-300'
                }`}
              >
                {type}
              </button>
            ))}
          </div>
        </div>

        {/* Project Grid */}
        <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
          {filteredProjects.length > 0 ? (
            filteredProjects.map((project) => (
              <div key={project.id} className="bg-white rounded-2xl shadow-sm border border-slate-100 hover:shadow-md transition-all overflow-hidden">
                <div className="p-5">
                  <div className="flex justify-between items-start mb-4">
                    <div>
                      <span className={`text-[10px] font-bold uppercase tracking-wider px-2 py-1 rounded-md mb-2 inline-block ${
                        project.type === 'Mainnet' ? 'bg-amber-100 text-amber-700' : 
                        project.type === 'Testnet' ? 'bg-blue-100 text-blue-700' :
                        project.type === 'Completed' ? 'bg-green-100 text-green-700' :
                        'bg-slate-100 text-slate-700'
                      }`}>
                        {project.type}
                      </span>
                      <h3 className="text-lg font-bold text-slate-800">{project.name}</h3>
                    </div>
                    <button onClick={() => deleteProject(project.id)} className="text-slate-300 hover:text-red-500 transition-colors">
                      <Trash2 size={18} />
                    </button>
                  </div>

                  <div className="space-y-3 mb-6">
                    <div className="flex items-center gap-2 text-sm text-slate-600">
                      <Wallet size={14} className="text-slate-400" />
                      <span className="font-mono bg-slate-50 px-2 py-0.5 rounded border border-slate-100">
                        {project.wallet || 'No wallet specified'}
                      </span>
                    </div>
                    {project.link && (
                      <a href={project.link} target="_blank" rel="noreferrer" className="flex items-center gap-2 text-sm text-indigo-600 hover:underline">
                        <ExternalLink size={14} /> Link Proyek
                      </a>
                    )}
                    <p className="text-sm text-slate-500 line-clamp-2 italic">
                      "{project.notes || 'Tidak ada catatan...'}"
                    </p>
                  </div>

                  {/* Task Section */}
                  <div className="border-t border-slate-50 pt-4">
                    <h4 className="text-xs font-bold text-slate-400 uppercase tracking-widest mb-3 flex items-center gap-2">
                      Checklist Tugas
                    </h4>
                    <div className="space-y-2 mb-4 max-h-40 overflow-y-auto pr-2">
                      {project.tasks.map((task, idx) => (
                        <div 
                          key={idx} 
                          onClick={() => toggleTask(project.id, idx)}
                          className={`flex items-center gap-3 p-2 rounded-lg cursor-pointer transition-colors ${task.completed ? 'bg-emerald-50 text-emerald-700' : 'hover:bg-slate-50 text-slate-700'}`}
                        >
                          {task.completed ? <CheckCircle2 size={18} className="text-emerald-500" /> : <Circle size={18} className="text-slate-300" />}
                          <span className={`text-sm ${task.completed ? 'line-through' : ''}`}>{task.title}</span>
                        </div>
                      ))}
                    </div>
                    
                    {/* Add Task Input */}
                    <form 
                      onSubmit={(e) => {
                        e.preventDefault();
                        const input = e.target.elements.taskInput;
                        addTask(project.id, input.value);
                        input.value = '';
                      }}
                      className="flex gap-2"
                    >
                      <input 
                        name="taskInput"
                        placeholder="Tambah tugas (e.g. Daily Swap)" 
                        className="flex-1 text-xs bg-slate-50 border-none rounded-lg px-3 py-2 outline-none focus:ring-1 focus:ring-indigo-300"
                      />
                      <button className="bg-slate-200 text-slate-600 p-2 rounded-lg hover:bg-indigo-100 hover:text-indigo-600 transition-colors">
                        <Plus size={14} />
                      </button>
                    </form>
                  </div>
                </div>
              </div>
            ))
          ) : (
            <div className="col-span-full py-20 text-center">
              <div className="bg-slate-100 w-16 h-16 rounded-full flex items-center justify-center mx-auto mb-4 text-slate-400">
                <AlertCircle size={32} />
              </div>
              <p className="text-slate-500 font-medium">Belum ada proyek. Ayo mulai garap!</p>
            </div>
          )}
        </div>
      </main>

      {/* Modal Tambah Proyek */}
      {isModalOpen && (
        <div className="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 flex items-center justify-center p-4">
          <div className="bg-white rounded-3xl w-full max-w-lg shadow-2xl overflow-hidden animate-in fade-in zoom-in duration-200">
            <div className="p-6 border-b border-slate-100 flex justify-between items-center">
              <h2 className="text-xl font-bold text-slate-800">Tambah Proyek Baru</h2>
              <button onClick={() => setIsModalOpen(false)} className="text-slate-400 hover:text-slate-600">
                <ChevronRight size={24} />
              </button>
            </div>
            <div className="p-6 space-y-4">
              <div>
                <label className="block text-sm font-semibold text-slate-700 mb-1">Nama Proyek</label>
                <input 
                  autoFocus
                  className="w-full px-4 py-2 rounded-xl border border-slate-200 outline-none focus:ring-2 focus:ring-indigo-500" 
                  placeholder="Contoh: Monad, Berachain..."
                  value={newProject.name}
                  onChange={(e) => setNewProject({...newProject, name: e.target.value})}
                />
              </div>
              <div className="grid grid-cols-2 gap-4">
                <div>
                  <label className="block text-sm font-semibold text-slate-700 mb-1">Tipe</label>
                  <select 
                    className="w-full px-4 py-2 rounded-xl border border-slate-200 outline-none focus:ring-2 focus:ring-indigo-500 bg-white"
                    value={newProject.type}
                    onChange={(e) => setNewProject({...newProject, type: e.target.value})}
                  >
                    <option>Mainnet</option>
                    <option>Testnet</option>
                    <option>Potential</option>
                    <option>Completed</option>
                  </select>
                </div>
                <div>
                  <label className="block text-sm font-semibold text-slate-700 mb-1">Nama Wallet/Alias</label>
                  <input 
                    className="w-full px-4 py-2 rounded-xl border border-slate-200 outline-none focus:ring-2 focus:ring-indigo-500" 
                    placeholder="EVM-1 / Rabby"
                    value={newProject.wallet}
                    onChange={(e) => setNewProject({...newProject, wallet: e.target.value})}
                  />
                </div>
              </div>
              <div>
                <label className="block text-sm font-semibold text-slate-700 mb-1">Link Ekosistem (Opsional)</label>
                <input 
                  className="w-full px-4 py-2 rounded-xl border border-slate-200 outline-none focus:ring-2 focus:ring-indigo-500" 
                  placeholder="https://..."
                  value={newProject.link}
                  onChange={(e) => setNewProject({...newProject, link: e.target.value})}
                />
              </div>
              <div>
                <label className="block text-sm font-semibold text-slate-700 mb-1">Catatan Tambahan</label>
                <textarea 
                  className="w-full px-4 py-2 rounded-xl border border-slate-200 outline-none focus:ring-2 focus:ring-indigo-500 h-24 resize-none" 
                  placeholder="Strategi atau syarat minimal..."
                  value={newProject.notes}
                  onChange={(e) => setNewProject({...newProject, notes: e.target.value})}
                ></textarea>
              </div>
              <button 
                onClick={handleAddProject}
                className="w-full bg-indigo-600 hover:bg-indigo-700 text-white font-bold py-3 rounded-xl transition-all shadow-lg shadow-indigo-100"
              >
                Simpan Proyek
              </button>
            </div>
          </div>
        </div>
      )}
    </div>
  );
};

export default App;
