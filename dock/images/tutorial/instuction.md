# Руководство по созданию Tower Defence игры на HTML, CSS и JavaScript

## 📋 Оглавление
1. [Подготовка проекта](#подготовка)
2. [Структура HTML](#html-структура)
3. [Стилизация игры](#css-стили)
4. [Базовая логика на JavaScript](#javascript-логика)
5. [Добавление врагов](#добавление-врагов)
6. [Система башен](#система-башен)
7. [Улучшение игры](#улучшение-игры)

---

## 1. Подготовка проекта {#подготовка}

Создайте структуру папок и файлов:
```
tower-defence-game/
├── index.html
├── style.css
├── game.js
└── assets/          (папка для изображений)
```

## 2. Структура HTML {#html-структура}

```html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tower Defence Game</title>
    <link rel="stylesheet" href="style.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
</head>
<body>
    <div class="game-container">
        <header class="game-header">
            <h1><i class="fas fa-fort-awesome"></i> Простая Tower Defence</h1>
            <div class="game-stats">
                <div class="stat">
                    <span class="stat-label">Жизни:</span>
                    <span id="lives" class="stat-value">20</span>
                </div>
                <div class="stat">
                    <span class="stat-label">Золото:</span>
                    <span id="gold" class="stat-value">100</span>
                </div>
                <div class="stat">
                    <span class="stat-label">Волна:</span>
                    <span id="wave" class="stat-value">1</span>
                </div>
            </div>
        </header>

        <div class="game-main">
            <div class="game-board">
                <canvas id="gameCanvas" width="800" height="600"></canvas>
                <div class="path-overlay"></div>
            </div>
            
            <aside class="game-sidebar">
                <div class="tower-shop">
                    <h2><i class="fas fa-store"></i> Магазин башен</h2>
                    <div class="tower-list">
                        <div class="tower-item" data-type="basic" data-cost="30">
                            <div class="tower-icon basic-tower"></div>
                            <div class="tower-info">
                                <h3>Базовая башня</h3>
                                <p>Урон: 5</p>
                                <p>Дальность: 150px</p>
                                <p class="tower-cost"><i class="fas fa-coins"></i> 30 золота</p>
                            </div>
                        </div>
                        <div class="tower-item" data-type="sniper" data-cost="80">
                            <div class="tower-icon sniper-tower"></div>
                            <div class="tower-info">
                                <h3>Снайперская</h3>
                                <p>Урон: 25</p>
                                <p>Дальность: 300px</p>
                                <p class="tower-cost"><i class="fas fa-coins"></i> 80 золота</p>
                            </div>
                        </div>
                        <div class="tower-item" data-type="splash" data-cost="60">
                            <div class="tower-icon splash-tower"></div>
                            <div class="tower-info">
                                <h3>Облачная</h3>
                                <p>Урон: 10</p>
                                <p>Дальность: 120px</p>
                                <p class="tower-cost"><i class="fas fa-coins"></i> 60 золота</p>
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="game-controls">
                    <button id="startWave" class="btn btn-primary">
                        <i class="fas fa-play"></i> Начать волну
                    </button>
                    <button id="upgradeTower" class="btn btn-secondary" disabled>
                        <i class="fas fa-level-up-alt"></i> Улучшить башню
                    </button>
                    <button id="sellTower" class="btn btn-danger" disabled>
                        <i class="fas fa-coins"></i> Продать башню
                    </button>
                </div>
                
                <div class="game-info">
                    <h3><i class="fas fa-info-circle"></i> Инструкция</h3>
                    <p>1. Выберите башню в магазине</p>
                    <p>2. Кликните на поле для установки</p>
                    <p>3. Нажмите "Начать волну"</p>
                    <p>4. Не дайте врагам дойти до конца!</p>
                </div>
            </aside>
        </div>
        
        <div class="game-messages" id="gameMessages">
            <p>Добро пожаловать в Tower Defence! Купите первую башню и начните волну.</p>
        </div>
    </div>

    <script src="game.js"></script>
</body>
</html>
```

## 3. Стилизация игры {#css-стили}

```css
/* Общие стили */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

body {
    background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
    color: #f1f1f1;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
}

.game-container {
    width: 100%;
    max-width: 1300px;
    background: rgba(25, 25, 35, 0.9);
    border-radius: 15px;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
    overflow: hidden;
    border: 1px solid #2a2a3e;
}

/* Шапка игры */
.game-header {
    background: linear-gradient(to right, #0f3460, #1a1a2e);
    padding: 15px 25px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-bottom: 3px solid #00adb5;
}

.game-header h1 {
    font-size: 1.8rem;
    color: #00adb5;
    display: flex;
    align-items: center;
    gap: 10px;
}

.game-stats {
    display: flex;
    gap: 25px;
}

.stat {
    display: flex;
    flex-direction: column;
    align-items: center;
    background: rgba(0, 0, 0, 0.3);
    padding: 8px 15px;
    border-radius: 8px;
    min-width: 80px;
    border: 1px solid #393e46;
}

.stat-label {
    font-size: 0.9rem;
    color: #bbbbbb;
}

.stat-value {
    font-size: 1.5rem;
    font-weight: bold;
    color: #ffd369;
}

/* Основная область игры */
.game-main {
    display: flex;
    padding: 20px;
    gap: 20px;
}

.game-board {
    flex: 3;
    position: relative;
    background-color: #222831;
    border-radius: 10px;
    overflow: hidden;
    border: 2px solid #393e46;
}

#gameCanvas {
    display: block;
    width: 100%;
    height: 100%;
    background-color: #1c1e26;
}

.path-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    background: 
        linear-gradient(to right, transparent 70%, rgba(15, 52, 96, 0.1) 100%),
        linear-gradient(to bottom, transparent 70%, rgba(15, 52, 96, 0.1) 100%);
}

/* Боковая панель */
.game-sidebar {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 20px;
    min-width: 300px;
}

.tower-shop {
    background: #2a2a3e;
    border-radius: 10px;
    padding: 20px;
    border: 1px solid #393e46;
}

.tower-shop h2 {
    color: #ffd369;
    margin-bottom: 15px;
    display: flex;
    align-items: center;
    gap: 10px;
    font-size: 1.3rem;
}

.tower-list {
    display: flex;
    flex-direction: column;
    gap: 15px;
}

.tower-item {
    background: #393e46;
    border-radius: 8px;
    padding: 12px;
    cursor: pointer;
    transition: all 0.3s ease;
    border: 2px solid transparent;
    display: flex;
    gap: 15px;
    align-items: center;
}

.tower-item:hover {
    background: #4a4f5a;
    border-color: #00adb5;
    transform: translateY(-3px);
}

.tower-item.selected {
    border-color: #ffd369;
    background: #4a4f5a;
}

.tower-icon {
    width: 50px;
    height: 50px;
    border-radius: 50%;
    flex-shrink: 0;
}

.basic-tower {
    background: radial-gradient(circle, #3498db, #1d6fa5);
    border: 3px solid #2980b9;
}

.sniper-tower {
    background: radial-gradient(circle, #9b59b6, #8e44ad);
    border: 3px solid #8e44ad;
}

.splash-tower {
    background: radial-gradient(circle, #e74c3c, #c0392b);
    border: 3px solid #c0392b;
}

.tower-info h3 {
    color: #f1f1f1;
    margin-bottom: 5px;
    font-size: 1.1rem;
}

.tower-info p {
    color: #bbbbbb;
    font-size: 0.85rem;
    margin: 2px 0;
}

.tower-cost {
    color: #ffd369 !important;
    font-weight: bold;
    display: flex;
    align-items: center;
    gap: 5px;
}

/* Элементы управления */
.game-controls {
    background: #2a2a3e;
    border-radius: 10px;
    padding: 20px;
    display: flex;
    flex-direction: column;
    gap: 12px;
    border: 1px solid #393e46;
}

.btn {
    padding: 12px;
    border: none;
    border-radius: 8px;
    font-size: 1rem;
    font-weight: bold;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 10px;
    transition: all 0.3s ease;
}

.btn-primary {
    background: linear-gradient(to right, #00adb5, #0097a7);
    color: white;
}

.btn-primary:hover:not(:disabled) {
    background: linear-gradient(to right, #0097a7, #00838f);
    transform: translateY(-2px);
}

.btn-secondary {
    background: linear-gradient(to right, #ffd369, #ffb142);
    color: #222831;
}

.btn-secondary:hover:not(:disabled) {
    background: linear-gradient(to right, #ffb142, #ff9f1a);
    transform: translateY(-2px);
}

.btn-danger {
    background: linear-gradient(to right, #e74c3c, #c0392b);
    color: white;
}

.btn-danger:hover:not(:disabled) {
    background: linear-gradient(to right, #c0392b, #a93226);
    transform: translateY(-2px);
}

.btn:disabled {
    opacity: 0.5;
    cursor: not-allowed;
    transform: none !important;
}

/* Информация об игре */
.game-info {
    background: #2a2a3e;
    border-radius: 10px;
    padding: 20px;
    border: 1px solid #393e46;
}

.game-info h3 {
    color: #00adb5;
    margin-bottom: 15px;
    display: flex;
    align-items: center;
    gap: 10px;
}

.game-info p {
    color: #bbbbbb;
    margin-bottom: 8px;
    font-size: 0.9rem;
    padding-left: 5px;
}

/* Сообщения игры */
.game-messages {
    background: #1a1a2e;
    padding: 15px 25px;
    border-top: 1px solid #393e46;
    color: #ffd369;
    font-size: 0.95rem;
    min-height: 60px;
    display: flex;
    align-items: center;
}

/* Адаптивность */
@media (max-width: 1100px) {
    .game-main {
        flex-direction: column;
    }
    
    .game-sidebar {
        min-width: 100%;
    }
    
    .tower-list {
        flex-direction: row;
        flex-wrap: wrap;
    }
    
    .tower-item {
        flex: 1;
        min-width: 200px;
    }
}

@media (max-width: 768px) {
    .game-header {
        flex-direction: column;
        gap: 15px;
        text-align: center;
    }
    
    .game-stats {
        width: 100%;
        justify-content: space-around;
    }
    
    .stat {
        min-width: 70px;
        padding: 6px 10px;
    }
}
```

## 4. Базовая логика на JavaScript {#javascript-логика}

```javascript
// game.js

// Инициализация игры
document.addEventListener('DOMContentLoaded', function() {
    // Получаем элементы DOM
    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');
    const livesElement = document.getElementById('lives');
    const goldElement = document.getElementById('gold');
    const waveElement = document.getElementById('wave');
    const startWaveButton = document.getElementById('startWave');
    const upgradeButton = document.getElementById('upgradeTower');
    const sellButton = document.getElementById('sellTower');
    const gameMessages = document.getElementById('gameMessages');
    const towerItems = document.querySelectorAll('.tower-item');
    
    // Состояние игры
    const gameState = {
        lives: 20,
        gold: 100,
        wave: 1,
        isWaveActive: false,
        selectedTowerType: null,
        selectedTowerCost: 0,
        selectedTower: null,
        towers: [],
        enemies: [],
        projectiles: [],
        cells: [],
        cellSize: 40,
        lastTime: 0,
        enemySpawnTimer: 0,
        enemySpawnInterval: 2000, // мс между появлением врагов
        enemiesInWave: 5,
        enemiesSpawned: 0
    };
    
    // Путь для врагов (точки в процентах от ширины/высоты canvas)
    const enemyPath = [
        {x: 0, y: 0.5},          // Начало пути (левая сторона)
        {x: 0.4, y: 0.5},        // Первая точка
        {x: 0.4, y: 0.2},        // Вверх
        {x: 0.7, y: 0.2},        // Вправо
        {x: 0.7, y: 0.7},        // Вниз
        {x: 1, y: 0.7}           // Конец пути (правая сторона)
    ];
    
    // Конвертируем процентные координаты в пиксельные
    function getPixelPath() {
        return enemyPath.map(point => ({
            x: point.x * canvas.width,
            y: point.y * canvas.height
        }));
    }
    
    // Инициализация игрового поля
    function initGameField() {
        // Создаем сетку клеток для размещения башен
        const cols = Math.floor(canvas.width / gameState.cellSize);
        const rows = Math.floor(canvas.height / gameState.cellSize);
        
        for (let x = 0; x < cols; x++) {
            for (let y = 0; y < rows; y++) {
                gameState.cells.push({
                    x: x * gameState.cellSize,
                    y: y * gameState.cellSize,
                    width: gameState.cellSize,
                    height: gameState.cellSize,
                    occupied: false,
                    tower: null
                });
            }
        }
        
        // Отмечаем клетки на пути врагов как занятые
        const path = getPixelPath();
        const pathWidth = gameState.cellSize * 1.5; // Ширина пути
        
        gameState.cells.forEach(cell => {
            // Проверяем, находится ли клетка рядом с путем
            const cellCenterX = cell.x + cell.width/2;
            const cellCenterY = cell.y + cell.height/2;
            
            for (let i = 0; i < path.length - 1; i++) {
                const start = path[i];
                const end = path[i+1];
                
                // Упрощенная проверка расстояния от точки до отрезка
                const distance = pointToSegmentDistance(
                    cellCenterX, cellCenterY, 
                    start.x, start.y, 
                    end.x, end.y
                );
                
                if (distance < pathWidth) {
                    cell.occupied = true;
                    break;
                }
            }
        });
    }
    
    // Функция расчета расстояния от точки до отрезка
    function pointToSegmentDistance(px, py, x1, y1, x2, y2) {
        const A = px - x1;
        const B = py - y1;
        const C = x2 - x1;
        const D = y2 - y1;

        const dot = A * C + B * D;
        const lenSq = C * C + D * D;
        let param = -1;
        
        if (lenSq !== 0) {
            param = dot / lenSq;
        }

        let xx, yy;

        if (param < 0) {
            xx = x1;
            yy = y1;
        } else if (param > 1) {
            xx = x2;
            yy = y2;
        } else {
            xx = x1 + param * C;
            yy = y1 + param * D;
        }

        const dx = px - xx;
        const dy = py - yy;
        
        return Math.sqrt(dx * dx + dy * dy);
    }
    
    // Выбор башни в магазине
    towerItems.forEach(item => {
        item.addEventListener('click', function() {
            // Снимаем выделение со всех башен
            towerItems.forEach(i => i.classList.remove('selected'));
            
            // Выделяем выбранную башню
            this.classList.add('selected');
            
            // Сохраняем тип и стоимость выбранной башни
            gameState.selectedTowerType = this.dataset.type;
            gameState.selectedTowerCost = parseInt(this.dataset.cost);
            
            showMessage(`Выбрана ${this.querySelector('h3').textContent}. Кликните на поле для установки.`);
            
            // Активируем режим размещения башни
            canvas.style.cursor = 'crosshair';
        });
    });
    
    // Размещение башни на поле
    canvas.addEventListener('click', function(e) {
        if (!gameState.selectedTowerType || gameState.isWaveActive) return;
        
        const rect = canvas.getBoundingClientRect();
        const x = e.clientX - rect.left;
        const y = e.clientY - rect.top;
        
        // Находим клетку, по которой кликнули
        const cell = gameState.cells.find(c => 
            x >= c.x && x <= c.x + c.width && 
            y >= c.y && y <= c.y + c.height
        );
        
        if (cell && !cell.occupied) {
            // Проверяем, хватает ли золота
            if (gameState.gold >= gameState.selectedTowerCost) {
                // Создаем башню
                const tower = createTower(
                    cell.x + cell.width/2, 
                    cell.y + cell.height/2, 
                    gameState.selectedTowerType
                );
                
                // Добавляем башню в игру
                gameState.towers.push(tower);
                
                // Обновляем клетку
                cell.occupied = true;
                cell.tower = tower;
                
                // Вычитаем стоимость
                gameState.gold -= gameState.selectedTowerCost;
                updateUI();
                
                showMessage(`Башня установлена! Осталось ${gameState.gold} золота.`);
                
                // Сбрасываем выбор
                resetTowerSelection();
            } else {
                showMessage(`Недостаточно золота! Нужно ${gameState.selectedTowerCost}, а у вас ${gameState.gold}.`);
            }
        }
    });
    
    // Создание башни
    function createTower(x, y, type) {
        const towerTypes = {
            basic: {
                damage: 5,
                range: 150,
                color: '#3498db',
                upgradeCost: 25,
                level: 1,
                fireRate: 1000 // мс между выстрелами
            },
            sniper: {
                damage: 25,
                range: 300,
                color: '#9b59b6',
                upgradeCost: 50,
                level: 1,
                fireRate: 2000
            },
            splash: {
                damage: 10,
                range: 120,
                color: '#e74c3c',
                upgradeCost: 40,
                level: 1,
                fireRate: 1500,
                splashRadius: 60
            }
        };
        
        const config = towerTypes[type];
        
        return {
            x, y,
            type,
            damage: config.damage,
            range: config.range,
            color: config.color,
            upgradeCost: config.upgradeCost,
            level: config.level,
            fireRate: config.fireRate,
            lastShot: 0,
            target: null,
            splashRadius: config.splashRadius || 0
        };
    }
    
    // Сброс выбора башни
    function resetTowerSelection() {
        towerItems.forEach(i => i.classList.remove('selected'));
        gameState.selectedTowerType = null;
        gameState.selectedTowerCost = 0;
        canvas.style.cursor = 'default';
    }
    
    // Создание врага
    function createEnemy() {
        const enemyTypes = [
            {health: 20, speed: 1, color: '#2ecc71', gold: 5},
            {health: 40, speed: 0.7, color: '#f39c12', gold: 10},
            {health: 60, speed: 0.5, color: '#e74c3c', gold: 15}
        ];
        
        // Выбираем тип врага в зависимости от волны
        let typeIndex = 0;
        if (gameState.wave > 3) typeIndex = 1;
        if (gameState.wave > 6) typeIndex = 2;
        
        const type = enemyTypes[typeIndex];
        
        // Начинаем с первой точки пути
        const path = getPixelPath();
        
        return {
            x: path[0].x,
            y: path[0].y,
            health: type.health + (gameState.wave * 2), // Увеличиваем здоровье с волнами
            maxHealth: type.health + (gameState.wave * 2),
            speed: type.speed,
            color: type.color,
            gold: type.gold + Math.floor(gameState.wave / 2),
            pathIndex: 0,
            path: path,
            reachedEnd: false
        };
    }
    
    // Обновление врагов
    function updateEnemies(deltaTime) {
        for (let i = gameState.enemies.length - 1; i >= 0; i--) {
            const enemy = gameState.enemies[i];
            
            // Проверяем, дошел ли враг до конца
            if (enemy.reachedEnd) {
                // Уменьшаем жизни
                gameState.lives--;
                updateUI();
                
                // Удаляем врага
                gameState.enemies.splice(i, 1);
                
                showMessage(`Враг достиг цели! Осталось ${gameState.lives} жизней.`);
                
                // Проверяем окончание игры
                if (gameState.lives <= 0) {
                    endGame(false);
                }
                
                continue;
            }
            
            // Двигаем врага по пути
            const targetPoint = enemy.path[enemy.pathIndex + 1];
            
            if (targetPoint) {
                const dx = targetPoint.x - enemy.x;
                const dy = targetPoint.y - enemy.y;
                const distance = Math.sqrt(dx * dx + dy * dy);
                
                if (distance < 2) {
                    // Достигли точки пути, переходим к следующей
                    enemy.pathIndex++;
                    
                    // Проверяем, дошли ли до конца пути
                    if (enemy.pathIndex >= enemy.path.length - 1) {
                        enemy.reachedEnd = true;
                    }
                } else {
                    // Двигаемся к точке
                    const moveDistance = enemy.speed * (deltaTime / 16);
                    enemy.x += (dx / distance) * moveDistance;
                    enemy.y += (dy / distance) * moveDistance;
                }
            }
        }
    }
    
    // Обновление башен
    function updateTowers(deltaTime) {
        gameState.towers.forEach(tower => {
            // Ищем цель для башни
            if (!tower.target || tower.target.health <= 0) {
                tower.target = findTargetForTower(tower);
            }
            
            // Стреляем по цели
            if (tower.target && tower.target.health > 0) {
                const currentTime = Date.now();
                
                if (currentTime - tower.lastShot > tower.fireRate) {
                    shootAtTarget(tower, tower.target);
                    tower.lastShot = currentTime;
                }
            }
        });
    }
    
    // Поиск цели для башни
    function findTargetForTower(tower) {
        let closestEnemy = null;
        let closestDistance = tower.range;
        
        for (const enemy of gameState.enemies) {
            const dx = enemy.x - tower.x;
            const dy = enemy.y - tower.y;
            const distance = Math.sqrt(dx * dx + dy * dy);
            
            if (distance < closestDistance) {
                closestEnemy = enemy;
                closestDistance = distance;
            }
        }
        
        return closestEnemy;
    }
    
    // Выстрел по цели
    function shootAtTarget(tower, target) {
        // Создаем снаряд
        const projectile = {
            x: tower.x,
            y: tower.y,
            target: target,
            damage: tower.damage,
            color: tower.color,
            speed: 5,
            splashRadius: tower.splashRadius
        };
        
        gameState.projectiles.push(projectile);
        
        // Эффект выстрела
        drawShotEffect(tower.x, tower.y, target.x, target.y, tower.color);
    }
    
    // Обновление снарядов
    function updateProjectiles() {
        for (let i = gameState.projectiles.length - 1; i >= 0; i--) {
            const projectile = gameState.projectiles[i];
            
            // Проверяем, существует ли еще цель
            if (!projectile.target || projectile.target.health <= 0) {
                gameState.projectiles.splice(i, 1);
                continue;
            }
            
            // Двигаем снаряд к цели
            const dx = projectile.target.x - projectile.x;
            const dy = projectile.target.y - projectile.y;
            const distance = Math.sqrt(dx * dx + dy * dy);
            
            if (distance < 10) {
                // Попадание!
                applyDamage(projectile);
                gameState.projectiles.splice(i, 1);
            } else {
                // Продолжаем движение
                projectile.x += (dx / distance) * projectile.speed;
                projectile.y += (dy / distance) * projectile.speed;
            }
        }
    }
    
    // Нанесение урона
    function applyDamage(projectile) {
        if (projectile.splashRadius > 0) {
            // Сплэш-урон по области
            for (const enemy of gameState.enemies) {
                const dx = enemy.x - projectile.target.x;
                const dy = enemy.y - projectile.target.y;
                const distance = Math.sqrt(dx * dx + dy * dy);
                
                if (distance < projectile.splashRadius) {
                    // Урон уменьшается с расстоянием
                    const damageMultiplier = 1 - (distance / projectile.splashRadius) * 0.5;
                    enemy.health -= projectile.damage * damageMultiplier;
                    
                    // Проверяем, убит ли враг
                    if (enemy.health <= 0) {
                        gameState.gold += enemy.gold;
                        showMessage(`Враг убит! Получено ${enemy.gold} золота.`);
                    }
                }
            }
        } else {
            // Обычный урон по одной цели
            projectile.target.health -= projectile.damage;
            
            // Проверяем, убит ли враг
            if (projectile.target.health <= 0) {
                gameState.gold += projectile.target.gold;
                showMessage(`Враг убит! Получено ${projectile.target.gold} золота.`);
            }
        }
        
        updateUI();
    }
    
    // Отрисовка игры
    function draw() {
        // Очищаем canvas
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        
        // Рисуем сетку
        drawGrid();
        
        // Рисуем путь врагов
        drawPath();
        
        // Рисуем башни
        gameState.towers.forEach(drawTower);
        
        // Рисуем врагов
        gameState.enemies.forEach(drawEnemy);
        
        // Рисуем снаряды
        gameState.projectiles.forEach(drawProjectile);
        
        // Рисуем информацию о выбранной башне
        if (gameState.selectedTower) {
            drawTowerRange(gameState.selectedTower);
        }
    }
    
    // Рисуем сетку
    function drawGrid() {
        ctx.strokeStyle = 'rgba(255, 255, 255, 0.1)';
        ctx.lineWidth = 1;
        
        // Вертикальные линии
        for (let x = 0; x <= canvas.width; x += gameState.cellSize) {
            ctx.beginPath();
            ctx.moveTo(x, 0);
            ctx.lineTo(x, canvas.height);
            ctx.stroke();
        }
        
        // Горизонтальные линии
        for (let y = 0; y <= canvas.height; y += gameState.cellSize) {
            ctx.beginPath();
            ctx.moveTo(0, y);
            ctx.lineTo(canvas.width, y);
            ctx.stroke();
        }
    }
    
    // Рисуем путь врагов
    function drawPath() {
        const path = getPixelPath();
        
        ctx.strokeStyle = 'rgba(46, 204, 113, 0.6)';
        ctx.lineWidth = 30;
        ctx.lineCap = 'round';
        ctx.lineJoin = 'round';
        
        ctx.beginPath();
        ctx.moveTo(path[0].x, path[0].y);
        
        for (let i = 1; i < path.length; i++) {
            ctx.lineTo(path[i].x, path[i].y);
        }
        
        ctx.stroke();
        
        // Рисуем границы пути
        ctx.strokeStyle = 'rgba(46, 204, 113, 0.9)';
        ctx.lineWidth = 2;
        ctx.stroke();
        
        // Рисуем точки пути
        for (let i = 0; i < path.length; i++) {
            ctx.fillStyle = i === 0 ? '#e74c3c' : (i === path.length - 1 ? '#3498db' : '#f1c40f');
            ctx.beginPath();
            ctx.arc(path[i].x, path[i].y, 8, 0, Math.PI * 2);
            ctx.fill();
            
            ctx.strokeStyle = '#2c3e50';
            ctx.lineWidth = 2;
            ctx.stroke();
        }
    }
    
    // Рисуем башню
    function drawTower(tower) {
        // Основание башни
        ctx.fillStyle = tower.color;
        ctx.beginPath();
        ctx.arc(tower.x, tower.y, 15, 0, Math.PI * 2);
        ctx.fill();
        
        // Обводка
        ctx.strokeStyle = '#2c3e50';
        ctx.lineWidth = 3;
        ctx.stroke();
        
        // Уровень башни
        ctx.fillStyle = '#fff';
        ctx.font = 'bold 12px Arial';
        ctx.textAlign = 'center';
        ctx.textBaseline = 'middle';
        ctx.fillText(tower.level.toString(), tower.x, tower.y);
        
        // Если башня выбрана, показываем радиус
        if (gameState.selectedTower === tower) {
            drawTowerRange(tower);
        }
    }
    
    // Рисуем радиус башни
    function drawTowerRange(tower) {
        ctx.strokeStyle = 'rgba(255, 255, 255, 0.3)';
        ctx.lineWidth = 2;
        ctx.beginPath();
        ctx.arc(tower.x, tower.y, tower.range, 0, Math.PI * 2);
        ctx.stroke();
        
        // Заливка радиуса
        ctx.fillStyle = 'rgba(255, 255, 255, 0.1)';
        ctx.fill();
    }
    
    // Рисуем врага
    function drawEnemy(enemy) {
        // Тело врага
        ctx.fillStyle = enemy.color;
        ctx.beginPath();
        ctx.arc(enemy.x, enemy.y, 12, 0, Math.PI * 2);
        ctx.fill();
        
        // Обводка
        ctx.strokeStyle = '#2c3e50';
        ctx.lineWidth = 2;
        ctx.stroke();
        
        // Полоска здоровья
        const healthWidth = 30;
        const healthPercent = enemy.health / enemy.maxHealth;
        
        // Фон полоски
        ctx.fillStyle = '#2c3e50';
        ctx.fillRect(enemy.x - healthWidth/2, enemy.y - 25, healthWidth, 5);
        
        // Здоровье
        ctx.fillStyle = healthPercent > 0.5 ? '#2ecc71' : (healthPercent > 0.25 ? '#f39c12' : '#e74c3c');
        ctx.fillRect(enemy.x - healthWidth/2, enemy.y - 25, healthWidth * healthPercent, 5);
        
        // Обводка полоски
        ctx.strokeStyle = '#34495e';
        ctx.lineWidth = 1;
        ctx.strokeRect(enemy.x - healthWidth/2, enemy.y - 25, healthWidth, 5);
    }
    
    // Рисуем снаряд
    function drawProjectile(projectile) {
        ctx.fillStyle = projectile.color;
        ctx.beginPath();
        ctx.arc(projectile.x, projectile.y, 5, 0, Math.PI * 2);
        ctx.fill();
        
        ctx.strokeStyle = '#2c3e50';
        ctx.lineWidth = 2;
        ctx.stroke();
    }
    
    // Эффект выстрела
    function drawShotEffect(fromX, fromY, toX, toY, color) {
        ctx.strokeStyle = color;
        ctx.lineWidth = 3;
        ctx.beginPath();
        ctx.moveTo(fromX, fromY);
        ctx.lineTo(toX, toY);
        ctx.stroke();
        
        // Исчезающий эффект
        setTimeout(() => {
            // Перерисовываем игру
            draw();
        }, 50);
    }
    
    // Обновление интерфейса
    function updateUI() {
        livesElement.textContent = gameState.lives;
        goldElement.textContent = gameState.gold;
        waveElement.textContent = gameState.wave;
        
        // Обновляем состояние кнопок
        upgradeButton.disabled = !gameState.selectedTower || gameState.isWaveActive;
        sellButton.disabled = !gameState.selectedTower || gameState.isWaveActive;
    }
    
    // Показ сообщений
    function showMessage(message) {
        gameMessages.innerHTML = `<p><i class="fas fa-info-circle"></i> ${message}</p>`;
        
        // Автоочистка через 3 секунды (кроме критических сообщений)
        if (!message.includes('жизней') && !message.includes('победили') && !message.includes('проиграли')) {
            setTimeout(() => {
                if (gameMessages.innerHTML.includes(message)) {
                    gameMessages.innerHTML = '<p>Игра продолжается...</p>';
                }
            }, 3000);
        }
    }
    
    // Начало волны
    startWaveButton.addEventListener('click', function() {
        if (!gameState.isWaveActive) {
            startWave();
        }
    });
    
    // Улучшение башни
    upgradeButton.addEventListener('click', function() {
        if (gameState.selectedTower && !gameState.isWaveActive) {
            upgradeTower(gameState.selectedTower);
        }
    });
    
    // Продажа башни
    sellButton.addEventListener('click', function() {
        if (gameState.selectedTower && !gameState.isWaveActive) {
            sellTower(gameState.selectedTower);
        }
    });
    
    // Выбор башни на поле (по клику)
    canvas.addEventListener('click', function(e) {
        if (gameState.isWaveActive) return;
        
        const rect = canvas.getBoundingClientRect();
        const x = e.clientX - rect.left;
        const y = e.clientY - rect.top;
        
        // Ищем башню по клику
        for (const tower of gameState.towers) {
            const dx = x - tower.x;
            const dy = y - tower.y;
            const distance = Math.sqrt(dx * dx + dy * dy);
            
            if (distance < 20) {
                // Выбираем башню
                gameState.selectedTower = tower;
                updateUI();
                showMessage(`Выбрана ${tower.type} башня (уровень ${tower.level}).`);
                return;
            }
        }
        
        // Если кликнули не по башне, снимаем выделение
        gameState.selectedTower = null;
        updateUI();
    });
    
    // Улучшение башни
    function upgradeTower(tower) {
        if (gameState.gold >= tower.upgradeCost) {
            gameState.gold -= tower.upgradeCost;
            
            // Улучшаем характеристики
            tower.level++;
            tower.damage = Math.floor(tower.damage * 1.5);
            tower.range = Math.floor(tower.range * 1.2);
            tower.upgradeCost = Math.floor(tower.upgradeCost * 1.8);
            
            // Улучшаем скорострельность
            tower.fireRate = Math.max(300, tower.fireRate * 0.9);
            
            updateUI();
            showMessage(`Башня улучшена до уровня ${tower.level}!`);
        } else {
            showMessage(`Недостаточно золота для улучшения! Нужно ${tower.upgradeCost}.`);
        }
    }
    
    // Продажа башни
    function sellTower(tower) {
        // Находим клетку с этой башней
        const cell = gameState.cells.find(c => c.tower === tower);
        
        if (cell) {
            // Возвращаем часть стоимости
            const sellPrice = Math.floor((tower.upgradeCost / 2) + (tower.level * 10));
            gameState.gold += sellPrice;
            
            // Удаляем башню
            const index = gameState.towers.indexOf(tower);
            if (index > -1) {
                gameState.towers.splice(index, 1);
            }
            
            // Освобождаем клетку
            cell.occupied = false;
            cell.tower = null;
            
            // Снимаем выделение
            gameState.selectedTower = null;
            
            updateUI();
            showMessage(`Башня продана за ${sellPrice} золота.`);
        }
    }
    
    // Начало волны врагов
    function startWave() {
        gameState.isWaveActive = true;
        gameState.enemiesSpawned = 0;
        gameState.enemySpawnTimer = 0;
        
        startWaveButton.disabled = true;
        startWaveButton.innerHTML = '<i class="fas fa-spinner fa-spin"></i> Волна в процессе';
        
        showMessage(`Волна ${gameState.wave} началась! Уничтожьте всех врагов.`);
    }
    
    // Завершение волны
    function endWave() {
        gameState.isWaveActive = false;
        gameState.wave++;
        
        // Награда за завершение волны
        const waveReward = 20 + gameState.wave * 5;
        gameState.gold += waveReward;
        
        startWaveButton.disabled = false;
        startWaveButton.innerHTML = '<i class="fas fa-play"></i> Начать волну';
        
        updateUI();
        showMessage(`Волна завершена! Получено ${waveReward} золота. Готовьтесь к волне ${gameState.wave}.`);
    }
    
    // Конец игры
    function endGame(isWin) {
        gameState.isWaveActive = false;
        
        if (isWin) {
            showMessage(`Поздравляем! Вы победили в игре! Уничтожено ${gameState.wave} волн врагов.`);
        } else {
            showMessage(`Игра окончена! Вы проиграли на волне ${gameState.wave}. Попробуйте еще раз!`);
        }
        
        // Блокируем кнопку начала волны
        startWaveButton.disabled = true;
        startWaveButton.innerHTML = '<i class="fas fa-redo"></i> Игра завершена';
        
        // Предлагаем перезапустить игру
        setTimeout(() => {
            if (confirm(isWin ? 
                "Вы победили! Хотите начать заново?" : 
                "Игра окончена. Хотите попробовать еще раз?")) {
                resetGame();
            }
        }, 1000);
    }
    
    // Сброс игры
    function resetGame() {
        gameState.lives = 20;
        gameState.gold = 100;
        gameState.wave = 1;
        gameState.isWaveActive = false;
        gameState.towers = [];
        gameState.enemies = [];
        gameState.projectiles = [];
        gameState.selectedTower = null;
        
        // Сбрасываем клетки
        gameState.cells.forEach(cell => {
            cell.occupied = false;
            cell.tower = null;
        });
        
        // Размечаем путь как занятый
        const path = getPixelPath();
        const pathWidth = gameState.cellSize * 1.5;
        
        gameState.cells.forEach(cell => {
            const cellCenterX = cell.x + cell.width/2;
            const cellCenterY = cell.y + cell.height/2;
            
            for (let i = 0; i < path.length - 1; i++) {
                const start = path[i];
                const end = path[i+1];
                
                const distance = pointToSegmentDistance(
                    cellCenterX, cellCenterY, 
                    start.x, start.y, 
                    end.x, end.y
                );
                
                if (distance < pathWidth) {
                    cell.occupied = true;
                    break;
                }
            }
        });
        
        updateUI();
        resetTowerSelection();
        showMessage("Игра сброшена! Купите первую башню и начните волну.");
    }
    
    // Игровой цикл
    function gameLoop(timestamp) {
        const deltaTime = timestamp - gameState.lastTime || 0;
        gameState.lastTime = timestamp;
        
        // Обновляем состояние игры
        if (gameState.isWaveActive) {
            // Спавн врагов
            if (gameState.enemiesSpawned < gameState.enemiesInWave + gameState.wave) {
                gameState.enemySpawnTimer += deltaTime;
                
                if (gameState.enemySpawnTimer >= gameState.enemySpawnInterval) {
                    gameState.enemies.push(createEnemy());
                    gameState.enemiesSpawned++;
                    gameState.enemySpawnTimer = 0;
                    
                    // Ускоряем спавн с каждой волной
                    gameState.enemySpawnInterval = Math.max(500, 2000 - (gameState.wave * 100));
                }
            }
            
            // Обновляем врагов
            updateEnemies(deltaTime);
            
            // Обновляем башни
            updateTowers(deltaTime);
            
            // Обновляем снаряды
            updateProjectiles();
            
            // Удаляем мертвых врагов
            for (let i = gameState.enemies.length - 1; i >= 0; i--) {
                if (gameState.enemies[i].health <= 0) {
                    gameState.enemies.splice(i, 1);
                }
            }
            
            // Проверяем завершение волны
            if (gameState.enemiesSpawned >= gameState.enemiesInWave + gameState.wave && 
                gameState.enemies.length === 0) {
                endWave();
            }
        }
        
        // Отрисовываем игру
        draw();
        
        // Проверяем победу (дошли до 10 волны)
        if (gameState.wave > 10) {
            endGame(true);
        }
        
        // Продолжаем игровой цикл
        requestAnimationFrame(gameLoop);
    }
    
    // Инициализация игры
    initGameField();
    updateUI();
    showMessage("Добро пожаловать в Tower Defence! Выберите башню в магазине и установите её на поле.");
    
    // Запуск игрового цикла
    requestAnimationFrame(gameLoop);
});
```

## 5. Добавление врагов {#добавление-врагов}

В коде выше уже реализована система врагов. Вот ключевые моменты:

1. **Создание врагов**: Функция `createEnemy()` создает врагов с разными характеристиками
2. **Движение по пути**: Враги следуют по предопределенному пути
3. **Волны**: С каждой волной увеличивается количество и сложность врагов
4. **Система здоровья**: У врагов есть полоска здоровья, которая отображается над ними

## 6. Система башен {#система-башен}

В игре есть три типа башен:

1. **Базовая башня**: Средний урон, средняя дальность, недорогая
2. **Снайперская башня**: Высокий урон, большая дальность, медленная скорострельность
3. **Облачная башня**: Средний урон, небольшая дальность, наносит урон по области

## 7. Улучшение игры {#улучшение-игры}

Для дальнейшего развития игры можно добавить:

1. **Больше типов врагов**: Летающие, бронированные, быстрые
2. **Специальные способности**: Замедление, оглушение, отравление
3. **Разные карты**: С уникальными путями и препятствиями
4. **Уровни сложности**: Легкий, нормальный, сложный
5. **Система достижений**: За уничтожение врагов, прохождение волн
6. **Звуковые эффекты**: Выстрелы, попадания, фоновую музыку

---

## 🚀 Запуск игры

1. Скопируйте все три блока кода в соответствующие файлы
2. Откройте файл `index.html` в любом современном браузере
3. Начните играть:
   - Выберите башню в магазине
   - Кликните на свободное место на поле
   - Нажмите "Начать волну"
   - Защищайте свой путь от врагов!

Это полное руководство позволяет создать работающую Tower Defence игру с нуля. Игра включает все основные элементы жанра: размещение башен, волны врагов, улучшение башен, экономику и прогресс.
