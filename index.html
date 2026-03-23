import React, { useState, useEffect, useRef } from 'react';
import { 
  Settings, 
  Plus, 
  Users, 
  Trophy, 
  X, 
  ChevronRight,
  Camera,
  AlertCircle,
  Clock,
  ClipboardList,
  UserPlus,
  LayoutDashboard,
  Move,
  Flame,
  Trash2,
  AlertTriangle,
  History,
  Percent,
  Activity,
  Crown
} from 'lucide-react';

// --- IndexedDB 工具函式 ---
const DB_NAME = 'BadmintonAppDB';
const DB_VERSION = 1;

const initDB = () => {
  return new Promise((resolve, reject) => {
    const request = indexedDB.open(DB_NAME, DB_VERSION);
    request.onupgradeneeded = (event) => {
      const db = event.target.result;
      if (!db.objectStoreNames.contains('players')) {
        db.createObjectStore('players', { keyPath: 'id' });
      }
      if (!db.objectStoreNames.contains('appState')) {
        db.createObjectStore('appState', { keyPath: 'key' });
      }
    };
    request.onsuccess = () => resolve(request.result);
    request.onerror = () => reject('IndexedDB 初始化失敗');
  });
};

const saveData = (storeName, data) => {
  return initDB().then(db => {
    return new Promise((resolve, reject) => {
      const transaction = db.transaction(storeName, 'readwrite');
      const store = transaction.objectStore(storeName);
      const request = store.put(data);
      request.onsuccess = () => resolve();
      request.onerror = () => reject();
    });
  });
};

const getAllData = (storeName) => {
  return initDB().then(db => {
    return new Promise((resolve, reject) => {
      const transaction = db.transaction(storeName, 'readonly');
      const store = transaction.objectStore(storeName);
      const request = store.getAll();
      request.onsuccess = () => resolve(request.result);
      request.onerror = () => reject();
    });
  });
};

const deleteData = (storeName, id) => {
  return initDB().then(db => {
    return new Promise((resolve, reject) => {
      const transaction = db.transaction(storeName, 'readwrite');
      const store = transaction.objectStore(storeName);
      const request = store.delete(id);
      request.onsuccess = () => resolve();
      request.onerror = () => reject();
    });
  });
};

// --- 工具函數 ---
const formatTime = (timestamp) => {
  if (!timestamp) return '';
  const date = new Date(timestamp);
  return `${date.getHours().toString().padStart(2, '0')}:${date.getMinutes().toString().padStart(2, '0')}`;
};

const calculateAge = (birthYear) => {
  if (!birthYear) return '未知';
  const currentYear = new Date().getFullYear();
  return currentYear - parseInt(birthYear);
};

const calculateWinRateDetail = (wins = 0, totalMatches = 0) => {
  if (totalMatches === 0) return '0%(0/0)';
  const rate = (wins / totalMatches) * 100;
  return `${rate.toFixed(1)}%(${wins}/${totalMatches})`;
};

const compressImage = (base64Str, maxWidth = 200, quality = 0.7) => {
  return new Promise((resolve) => {
    const img = new Image();
    img.src = base64Str;
    img.onload = () => {
      const canvas = document.createElement('canvas');
      let width = img.width;
      let height = img.height;
      if (width > maxWidth) {
        height = (maxWidth / width) * height;
        width = maxWidth;
      }
      canvas.width = width;
      canvas.height = height;
      const ctx = canvas.getContext('2d');
      ctx.drawImage(img, 0, 0, width, height);
      resolve(canvas.toDataURL('image/jpeg', quality));
    };
  });
};

export default function App() {
  const [loading, setLoading] = useState(true);
  const [view, setView] = useState('main'); 
  const fileInputRef = useRef(null);
  const editFileInputRef = useRef(null);
  const [errorMsg, setErrorMsg] = useState('');
  
  const [players, setPlayers] = useState([]);
  const [activeGroup, setActiveGroup] = useState(null);
  const [matches, setMatches] = useState([]);

  const [showAddModal, setShowAddModal] = useState(false);
  const [showHistoryModal, setShowHistoryModal] = useState(false);
  const [newPlayerData, setNewPlayerData] = useState({
    name: '', gender: '男', birthYear: '', avatar: ''
  });
  const [selectedPlayer, setSelectedPlayer] = useState(null);
  const [showConfirmEnd, setShowConfirmEnd] = useState(false);
  const [playerToDelete, setPlayerToDelete] = useState(null); 
  
  // 拖移相關狀態
  const [draggedPlayerId, setDraggedPlayerId] = useState(null);
  const [dragSource, setDragSource] = useState(null);

  useEffect(() => {
    const loadAll = async () => {
      try {
        const pList = await getAllData('players');
        setPlayers(pList);
        const stateItems = await getAllData('appState');
        const gameState = stateItems.find(i => i.key === 'current');
        if (gameState) {
          setActiveGroup(gameState.activeGroup || null);
          setMatches(gameState.matches || []);
        }
      } catch (err) {
        setErrorMsg("載入本地資料失敗");
      } finally {
        setLoading(false);
      }
    };
    loadAll();
  }, []);

  const syncGameState = async (newGroup, newMatches) => {
    try {
      const g = newGroup !== undefined ? newGroup : activeGroup;
      const m = newMatches !== undefined ? newMatches : matches;
      setActiveGroup(g);
      setMatches(m);
      await saveData('appState', {
        key: 'current',
        activeGroup: g,
        matches: m,
        updatedAt: Date.now()
      });
    } catch (err) {
      setErrorMsg("儲存狀態失敗");
    }
  };

  const handleEndSession = async () => {
    try {
      const stats = {};
      matches.forEach(m => {
        const allInMatch = [...m.teamA, ...m.teamB];
        allInMatch.forEach(pid => {
          if (!stats[pid]) stats[pid] = { wins: 0, points: 0 };
          stats[pid].points += 1;
          if (m.winners.includes(pid)) stats[pid].wins += 1;
        });
      });
      const now = Date.now();
      const updatedPlayers = [...players];
      for (const pid in stats) {
        const idx = updatedPlayers.findIndex(p => p.id === pid);
        if (idx !== -1) {
          updatedPlayers[idx] = {
            ...updatedPlayers[idx],
            wins: (updatedPlayers[idx].wins || 0) + stats[pid].wins,
            points: (updatedPlayers[idx].points || 0) + stats[pid].points,
            firstMatchTime: updatedPlayers[idx].firstMatchTime || now
          };
          await saveData('players', updatedPlayers[idx]);
        }
      }
      setPlayers(updatedPlayers);
      await syncGameState(null, []);
      setShowConfirmEnd(false);
    } catch (err) { setErrorMsg("結算儲存失敗"); }
  };

  const handleImageChange = async (e, mode) => {
    const file = e.target.files[0];
    if (file) {
      const reader = new FileReader();
      reader.onloadend = async () => {
        const compressed = await compressImage(reader.result);
        if (mode === 'add') setNewPlayerData(prev => ({ ...prev, avatar: compressed }));
        else if (mode === 'edit') setSelectedPlayer(prev => ({ ...prev, avatar: compressed }));
      };
      reader.readAsDataURL(file);
    }
  };

  const startGroup = (selectedIds) => {
    const newGroup = {
      id: Date.now().toString(),
      players: selectedIds,
      queue: [...selectedIds],
      courts: {
        1: { teamA: [], teamB: [] },
        2: { teamA: [], teamB: [] },
        3: { teamA: [], teamB: [] },
        4: { teamA: [], teamB: [] }
      }
    };
    syncGameState(newGroup, []);
    setView('main');
  };

  const fillSpot = (courtId, teamSide, playerId = null) => {
    if (!activeGroup) return;
    let targetPlayerId = playerId;
    let updatedQueue = [...activeGroup.queue];
    if (!targetPlayerId) {
      if (updatedQueue.length === 0) return;
      targetPlayerId = updatedQueue.shift();
    } else {
      updatedQueue = updatedQueue.filter(id => id !== targetPlayerId);
    }
    const nextCourts = { ...activeGroup.courts };
    const currentTeam = nextCourts[courtId][teamSide];
    if (currentTeam.length >= 2) return;
    nextCourts[courtId][teamSide] = [...currentTeam, targetPlayerId];
    syncGameState({ ...activeGroup, queue: updatedQueue, courts: nextCourts });
  };

  const removePlayerFromCourt = (courtId, teamSide, playerId) => {
    if (!activeGroup) return;
    const nextCourts = { ...activeGroup.courts };
    nextCourts[courtId][teamSide] = nextCourts[courtId][teamSide].filter(id => id !== playerId);
    const updatedQueue = [...activeGroup.queue, playerId];
    syncGameState({ ...activeGroup, queue: updatedQueue, courts: nextCourts });
  };

  const finishMatch = async (courtId, winnerSide) => {
    if (!activeGroup) return;
    const court = activeGroup.courts[courtId];
    if (court.teamA.length < 2 || court.teamB.length < 2) return;
    const winners = winnerSide === 'A' ? court.teamA : court.teamB;
    const matchData = {
      id: Date.now().toString(),
      endTime: Date.now(),
      teamA: court.teamA,
      teamB: court.teamB,
      winners: winners,
      winnerSide: winnerSide,
      courtNum: courtId
    };
    const newMatches = [matchData, ...matches];
    const playersLeaving = [...court.teamA, ...court.teamB];
    const updatedQueue = [...activeGroup.queue, ...playersLeaving];
    const nextCourts = { ...activeGroup.courts };
    nextCourts[courtId] = { teamA: [], teamB: [] };
    syncGameState({ ...activeGroup, queue: updatedQueue, courts: nextCourts }, newMatches);
  };

  const handleQueueDragStart = (e, playerId, index) => {
    setDraggedPlayerId(playerId);
    setDragSource({ type: 'queue', index });
    e.dataTransfer.setData("playerId", playerId);
    e.dataTransfer.setData("sourceIndex", index);
    e.dataTransfer.effectAllowed = "move";
  };

  const handleQueueDrop = (e, dropIndex) => {
    e.preventDefault();
    if (!dragSource || dragSource.type !== 'queue') return;
    const sourceIndex = dragSource.index;
    if (sourceIndex === dropIndex) return;
    const newQueue = [...activeGroup.queue];
    const [movedId] = newQueue.splice(sourceIndex, 1);
    newQueue.splice(dropIndex, 0, movedId);
    syncGameState({ ...activeGroup, queue: newQueue });
    setDragSource(null);
    setDraggedPlayerId(null);
  };

  const handleCourtDrop = (e, courtId, teamSide) => {
    e.preventDefault();
    const pid = e.dataTransfer.getData("playerId");
    if (!pid) return;
    fillSpot(courtId, teamSide, pid);
    setDragSource(null);
    setDraggedPlayerId(null);
  };

  const handleConfirmAddPlayer = async () => {
    if (!newPlayerData.name.trim()) return setErrorMsg("請輸入球員姓名");
    const playerId = crypto.randomUUID();
    const defaultAvatar = `https://api.dicebear.com/7.x/avataaars/svg?seed=${encodeURIComponent(newPlayerData.name.trim())}`;
    const newDocData = {
      id: playerId,
      name: newPlayerData.name.trim(),
      gender: newPlayerData.gender,
      birthYear: newPlayerData.birthYear || "1990",
      points: 0, wins: 0,
      avatar: newPlayerData.avatar || defaultAvatar,
      createdAt: Date.now()
    };
    try {
      await saveData('players', newDocData);
      setPlayers(prev => [newDocData, ...prev]);
      setShowAddModal(false);
      setNewPlayerData({ name: '', gender: '男', birthYear: '', avatar: '' });
    } catch (err) { setErrorMsg("新增失敗"); }
  };

  const handleUpdatePlayer = async () => {
    if (!selectedPlayer) return;
    try {
      await saveData('players', selectedPlayer);
      setPlayers(prev => prev.map(p => p.id === selectedPlayer.id ? selectedPlayer : p));
      setSelectedPlayer(null);
    } catch (err) { setErrorMsg("更新失敗"); }
  };

  const handleDeletePlayer = async (p) => {
    setPlayerToDelete(p);
  };

  const confirmDeletePlayer = async () => {
    if (!playerToDelete) return;
    try {
      await deleteData('players', playerToDelete.id);
      setPlayers(players.filter(p => p.id !== playerToDelete.id));
      setPlayerToDelete(null);
    } catch (err) { setErrorMsg("刪除失敗"); }
  };

  if (loading) return (
    <div className="h-screen flex flex-col items-center justify-center bg-gray-50 text-blue-600 gap-4">
      <div className="w-10 h-10 border-4 border-blue-200 border-t-blue-600 rounded-full animate-spin"></div>
      <div className="font-bold">載入本地資料中...</div>
    </div>
  );

  return (
    <div className="flex flex-col h-screen bg-gray-50 max-w-md mx-auto relative shadow-2xl font-sans text-gray-800 overflow-hidden">
      {/* Header */}
      <div className="flex justify-between items-center p-4 bg-blue-600 text-white shadow-md shrink-0 z-10">
        <h1 className="text-xl font-bold flex items-center gap-2 cursor-pointer" onClick={() => setView('main')}>
          <Trophy size={24} /> 羽球助手
        </h1>
        <div className="flex gap-2">
          {activeGroup && <button onClick={() => setShowHistoryModal(true)} className="bg-white/20 p-2 rounded-full active:scale-95"><ClipboardList size={20} /></button>}
          {!activeGroup ? (
            <button onClick={() => setView('grouping')} className="bg-orange-500 p-2 rounded-full active:scale-95 flex items-center justify-center animate-pulse"><Flame size={20} /></button>
          ) : (
            <button onClick={() => setShowConfirmEnd(true)} className="bg-red-500 p-2 rounded-full active:scale-95"><X size={20} /></button>
          )}
          <button onClick={() => setView('settings')} className="bg-blue-700 p-2 rounded-full active:scale-95"><Settings size={20} /></button>
        </div>
      </div>

      {errorMsg && (
        <div className="absolute top-16 left-4 right-4 bg-red-600 text-white p-3 rounded-xl shadow-lg z-[200] flex items-center gap-2 text-sm">
          <AlertCircle size={18} /> <span className="flex-1">{errorMsg}</span>
          <button onClick={() => setErrorMsg('')}><X size={14}/></button>
        </div>
      )}
      
      {view === 'settings' ? (
        <div className="flex-1 overflow-y-auto p-4 pb-20">
          <div className="flex justify-between items-center mb-4">
            <h3 className="font-bold flex items-center gap-2"><Users size={18}/> 人員管理 ({players.length})</h3>
            <div className="flex gap-2">
              {activeGroup && <button onClick={() => setView('main')} className="bg-gray-200 text-gray-700 text-xs px-4 py-2 rounded-full font-bold flex items-center gap-1 shadow-sm"><LayoutDashboard size={14} /> 返回</button>}
              {!activeGroup && <button onClick={() => setShowAddModal(true)} className="bg-blue-600 text-white text-xs px-5 py-2 rounded-full font-bold shadow-sm">+ 新增</button>}
            </div>
          </div>
          <div className="space-y-3">
            {players.sort((a,b) => (b.createdAt || 0) - (a.createdAt || 0)).map(p => (
              <div key={p.id} className="bg-white rounded-2xl flex items-center gap-4 shadow-sm border border-gray-100 overflow-hidden group">
                <div className="p-4 flex flex-1 items-center gap-4 cursor-pointer" onClick={() => setSelectedPlayer(p)}>
                  <img src={p.avatar} className="w-12 h-12 rounded-full object-cover bg-gray-100 border" />
                  <div className="flex-1 min-w-0">
                    <div className="font-bold text-gray-900 truncate">{p.name}</div>
                    <div className="text-[10px] text-gray-400 flex flex-wrap gap-1.5 mt-1 items-center">
                      <span className="bg-gray-100 px-1.5 py-0.5 rounded text-gray-600">{p.gender}</span>
                      <div className="flex gap-1 items-center bg-green-50 px-1.5 py-0.5 rounded text-green-600 font-bold"><Percent size={10} /> {calculateWinRateDetail(p.wins, p.points)}</div>
                      <div className="flex gap-1 items-center bg-amber-50 px-1.5 py-0.5 rounded text-amber-600 font-bold"><Trophy size={10} /> {p.wins || 0} 勝</div>
                    </div>
                  </div>
                </div>
                {!activeGroup && (
                  <button 
                    onClick={(e) => { e.stopPropagation(); handleDeletePlayer(p); }} 
                    className="p-4 text-gray-300 hover:text-red-500 transition-colors"
                  >
                    <Trash2 size={18} />
                  </button>
                )}
              </div>
            ))}
          </div>
        </div>
      ) : view === 'grouping' ? (
        <div className="flex-1 overflow-y-auto p-4 pb-20">
          <h2 className="text-lg font-bold mb-4">選擇上場名單</h2>
          <div className="bg-white rounded-2xl shadow-sm border mb-6 max-h-[60vh] overflow-y-auto">
            {players.map(p => (
              <label key={p.id} className="p-4 flex items-center gap-4 border-b last:border-0 cursor-pointer">
                <input type="checkbox" className="w-5 h-5 rounded text-blue-600" id={`chk-${p.id}`} />
                <img src={p.avatar} className="w-8 h-8 rounded-full border object-cover" />
                <div className="flex-1 font-bold text-sm">{p.name} <span className="text-xs text-gray-400 ml-1 font-normal">({calculateAge(p.birthYear)}歲)</span></div>
              </label>
            ))}
          </div>
          <button onClick={() => {
            const selectedIds = players.filter(p => document.getElementById(`chk-${p.id}`).checked).map(p => p.id);
            if (selectedIds.length < 4) return setErrorMsg("請至少選擇 4 人");
            startGroup(selectedIds);
          }} className="w-full bg-blue-600 text-white p-4 rounded-2xl font-bold shadow-lg">確定開團</button>
        </div>
      ) : (
        <div className="flex-1 flex flex-col overflow-hidden">
          {!activeGroup ? (
            <div className="flex-1 flex flex-col items-center justify-center p-10 text-center text-gray-400">
              <Users size={48} className="mb-4 opacity-20" />
              <p>點擊上方火焰圖示開始開打</p>
            </div>
          ) : (
            <>
              <div className="flex-1 p-4 overflow-y-auto pb-48 scroll-smooth">
                <div className="grid grid-cols-1 gap-6">
                  {[1, 2, 3, 4].map(courtId => {
                    const court = activeGroup.courts[courtId];
                    const isFull = court.teamA.length === 2 && court.teamB.length === 2;
                    return (
                      <div key={courtId} className="border-2 rounded-2xl p-4 bg-white border-blue-100 shadow-sm">
                        <div className="text-xs font-black text-blue-600 uppercase mb-3">球場 {courtId}</div>
                        <div className="flex gap-2 mb-4">
                          {['teamA', 'teamB'].map((side) => (
                            <React.Fragment key={side}>
                              <div className="flex-1 flex flex-col gap-2">
                                {[0, 1].map(idx => (
                                  <div key={`${side}-${idx}`} 
                                    onDragOver={(e) => e.preventDefault()} 
                                    onDrop={(e) => handleCourtDrop(e, courtId, side)} 
                                    className={`h-12 border-2 border-dashed rounded-xl flex items-center p-1 ${court[side][idx] ? 'border-transparent' : 'border-gray-100'}`}
                                  >
                                    {court[side][idx] ? (
                                      <div className={`w-full h-full rounded-lg flex items-center gap-2 p-1 ${side === 'teamA' ? 'bg-blue-50' : 'bg-orange-50'}`}>
                                        <img src={players.find(p => p.id === court[side][idx])?.avatar} className="w-8 h-8 rounded-full object-cover" />
                                        <span className="text-[10px] font-bold truncate flex-1">{players.find(p => p.id === court[side][idx])?.name}</span>
                                        <button onClick={() => removePlayerFromCourt(courtId, side, court[side][idx])} className="text-gray-400"><X size={14}/></button>
                                      </div>
                                    ) : ( <button onClick={() => fillSpot(courtId, side)} className="w-full h-full flex items-center justify-center text-gray-200"><UserPlus size={18} /></button> )}
                                  </div>
                                ))}
                              </div>
                              {side === 'teamA' && <div className="flex items-center text-gray-200 font-black italic text-xs">VS</div>}
                            </React.Fragment>
                          ))}
                        </div>
                        {isFull && (
                          <div className="flex gap-2">
                            <button onClick={() => finishMatch(courtId, 'A')} className="flex-1 bg-blue-600 text-white text-xs py-2 rounded-xl font-bold">A 勝</button>
                            <button onClick={() => finishMatch(courtId, 'B')} className="flex-1 bg-orange-500 text-white text-xs py-2 rounded-xl font-bold">B 勝</button>
                          </div>
                        )}
                      </div>
                    );
                  })}
                </div>
              </div>
              
              <div className="absolute bottom-0 left-0 right-0 bg-white p-4 border-t h-44 flex flex-col gap-2 z-50 shadow-[0_-4px_20px_rgba(0,0,0,0.05)]">
                <div className="flex justify-between items-center">
                  <span className="text-xs font-bold text-gray-400 flex items-center gap-1"><Move size={12}/> 隊伍佇列 ({activeGroup.queue.length})</span>
                  <span className="text-[10px] text-gray-300">拖動可調整順序</span>
                </div>
                <div className="flex gap-4 items-center overflow-x-auto pb-2 no-scrollbar">
                  {activeGroup.queue.map((pid, idx) => {
                    const p = players.find(x => x.id === pid);
                    return (
                      <div key={`${pid}-${idx}`} 
                        draggable 
                        onDragStart={(e) => handleQueueDragStart(e, pid, idx)}
                        onDragOver={(e) => e.preventDefault()}
                        onDrop={(e) => handleQueueDrop(e, idx)}
                        className={`shrink-0 flex flex-col items-center w-14 transition-all ${draggedPlayerId === pid ? 'opacity-30 scale-90' : 'cursor-grab active:cursor-grabbing'}`}
                      >
                        <div className="relative">
                          <img src={p?.avatar} className="w-12 h-12 rounded-full mb-1 border-2 border-white shadow-md object-cover bg-gray-50" />
                          <div className="absolute -top-1 -right-1 bg-gray-400 text-white text-[8px] w-4 h-4 rounded-full flex items-center justify-center border border-white font-bold">{idx + 1}</div>
                        </div>
                        <span className="text-[10px] font-bold truncate w-full text-center">{p?.name}</span>
                      </div>
                    );
                  })}
                </div>
              </div>
            </>
          )}
        </div>
      )}

      {/* Modals */}
      {playerToDelete && (
        <div className="fixed inset-0 bg-black/70 flex items-center justify-center p-8 z-[500] backdrop-blur-sm">
          <div className="bg-white rounded-[2.5rem] p-8 w-full max-w-xs text-center">
            <AlertTriangle size={32} className="mx-auto text-red-500 mb-6" />
            <h3 className="text-xl font-bold mb-4">確定刪除 {playerToDelete.name}？</h3>
            <p className="text-sm text-gray-400 mb-8">此操作將無法恢復該球員的所有勝場紀錄。</p>
            <button onClick={confirmDeletePlayer} className="w-full bg-red-600 text-white py-4 rounded-2xl font-bold mb-4 shadow-lg active:scale-95 transition-transform">確認刪除</button>
            <button onClick={() => setPlayerToDelete(null)} className="text-gray-400 font-bold">取消</button>
          </div>
        </div>
      )}

      {showHistoryModal && (
        <div className="fixed inset-0 bg-black/60 flex items-center justify-center p-4 z-[400]">
          <div className="bg-white rounded-[2rem] w-full max-w-sm h-[80vh] flex flex-col shadow-2xl">
            <div className="p-5 border-b flex justify-between items-center"><h2 className="font-bold">比賽歷史</h2><button onClick={() => setShowHistoryModal(false)}><X size={20}/></button></div>
            <div className="flex-1 overflow-y-auto p-4 space-y-4">
              {matches.map((m, idx) => (
                <div key={m.id} className="bg-gray-50 rounded-2xl p-4 border text-[11px] relative">
                  <div className="flex justify-between mb-2 text-gray-400"><span>#{matches.length - idx} 球場 {m.courtNum}</span><span>{formatTime(m.endTime)}</span></div>
                  <div className="flex gap-2">
                    <div className={`flex-1 p-2 rounded-xl flex items-center justify-center gap-1 border ${m.winnerSide === 'A' ? 'bg-blue-100 border-blue-200 font-bold' : 'bg-white border-gray-100'}`}>
                      {m.winnerSide === 'A' && <Crown size={12} className="text-amber-500" />}
                      {m.teamA.map(pid => players.find(p => p.id === pid)?.name).join(' & ')}
                    </div>
                    <div className="flex items-center font-black italic text-gray-300">VS</div>
                    <div className={`flex-1 p-2 rounded-xl flex items-center justify-center gap-1 border ${m.winnerSide === 'B' ? 'bg-orange-100 border-orange-200 font-bold' : 'bg-white border-gray-100'}`}>
                      {m.winnerSide === 'B' && <Crown size={12} className="text-amber-500" />}
                      {m.teamB.map(pid => players.find(p => p.id === pid)?.name).join(' & ')}
                    </div>
                  </div>
                </div>
              ))}
            </div>
          </div>
        </div>
      )}

      {(showAddModal || selectedPlayer) && (
        <div className="fixed inset-0 bg-black/60 flex items-center justify-center p-6 z-[300]">
          <div className="bg-white rounded-[2rem] p-8 w-full max-w-sm shadow-2xl">
            <h2 className="text-xl font-bold mb-6 text-center">{showAddModal ? '新增球員' : '修改資料'}</h2>
            <div className="space-y-4 text-center">
              <div className="relative inline-block group" onClick={() => (showAddModal ? fileInputRef : editFileInputRef).current.click()}>
                <img src={showAddModal ? (newPlayerData.avatar || `https://api.dicebear.com/7.x/avataaars/svg?seed=p`) : selectedPlayer.avatar} className="w-24 h-24 rounded-full border-4 border-gray-50 object-cover mx-auto cursor-pointer shadow-md" />
                <div className="absolute bottom-0 right-0 bg-blue-600 text-white p-1.5 rounded-full border-2 border-white"><Camera size={14} /></div>
                <input type="file" accept="image/*" ref={showAddModal ? fileInputRef : editFileInputRef} className="hidden" onChange={(e) => handleImageChange(e, showAddModal ? 'add' : 'edit')} />
              </div>
              <input className="w-full border p-3 rounded-xl font-bold focus:ring-2 focus:ring-blue-100 outline-none" value={showAddModal ? newPlayerData.name : selectedPlayer.name} onChange={(e) => showAddModal ? setNewPlayerData({...newPlayerData, name: e.target.value}) : setSelectedPlayer({...selectedPlayer, name: e.target.value})} placeholder="姓名" />
              <div className="flex gap-4">
                <select className="flex-1 border p-3 rounded-xl outline-none" value={showAddModal ? newPlayerData.gender : selectedPlayer.gender} onChange={(e) => showAddModal ? setNewPlayerData({...newPlayerData, gender: e.target.value}) : setSelectedPlayer({...selectedPlayer, gender: e.target.value})}><option value="男">男</option><option value="女">女</option></select>
                <input type="number" className="flex-1 border p-3 rounded-xl outline-none" placeholder="西元出生年" value={showAddModal ? newPlayerData.birthYear : selectedPlayer.birthYear} onChange={(e) => showAddModal ? setNewPlayerData({...newPlayerData, birthYear: e.target.value}) : setSelectedPlayer({...selectedPlayer, birthYear: e.target.value})} />
              </div>
              <div className="flex gap-3 pt-4">
                <button onClick={() => showAddModal ? setShowAddModal(false) : setSelectedPlayer(null)} className="flex-1 py-3 text-gray-400 font-bold">取消</button>
                <button onClick={showAddModal ? handleConfirmAddPlayer : handleUpdatePlayer} className="flex-1 py-3 rounded-2xl bg-blue-600 text-white font-bold shadow-lg">儲存</button>
              </div>
            </div>
          </div>
        </div>
      )}

      {showConfirmEnd && (
        <div className="fixed inset-0 bg-black/80 flex items-center justify-center p-8 z-[400]">
          <div className="bg-white rounded-[2rem] p-8 w-full max-w-xs text-center shadow-2xl">
            <h3 className="text-xl font-bold mb-4">結算本次活動？</h3>
            <p className="text-sm text-gray-400 mb-8">系統將統計勝場並更新至球員生涯紀錄，同時清空當前場地與歷史。</p>
            <button onClick={handleEndSession} className="w-full bg-red-500 text-white py-4 rounded-xl font-bold mb-3 shadow-lg active:scale-95">結算並關團</button>
            <button onClick={() => setShowConfirmEnd(false)} className="w-full text-gray-400 py-2 text-sm font-bold">返回</button>
          </div>
        </div>
      )}
    </div>
  );
}