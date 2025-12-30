#include <windows.h>
#include <iostream>

// Функция для смены раскладки клавиатуры
void ChangeKeyboardLayout();

LRESULT CALLBACK KeyboardProc(int nCode, WPARAM wParam, LPARAM lParam) {
    if (nCode == HC_ACTION) {
        KBDLLHOOKSTRUCT* pKeyBoard = (KBDLLHOOKSTRUCT*)lParam;

        // Проверяем, нажата ли клавиша Caps Lock
        if (wParam == WM_KEYDOWN) {
            if (pKeyBoard->vkCode == VK_CAPITAL) {
                // Переключаем язык
                ChangeKeyboardLayout(); // Вызываем функцию для смены раскладки
                return 1; // Блокируем нажатие клавиши Caps Lock
            }
        }
    }
    return CallNextHookEx(NULL, nCode, wParam, lParam);
}

void ChangeKeyboardLayout() {
    // Отправляем комбинацию клавиш Alt + Shift для смены раскладки
    INPUT inputs[4] = {};

    // Нажимаем Alt
    inputs[0].type = INPUT_KEYBOARD;
    inputs[0].ki.wVk = VK_MENU; // VK_MENU - это Alt

    // Нажимаем Shift
    inputs[1].type = INPUT_KEYBOARD;
    inputs[1].ki.wVk = VK_SHIFT; // VK_SHIFT - это Shift

    // Отпускаем Shift
    inputs[2].type = INPUT_KEYBOARD;
    inputs[2].ki.wVk = VK_SHIFT;
    inputs[2].ki.dwFlags = KEYEVENTF_KEYUP;

    // Отпускаем Alt
    inputs[3].type = INPUT_KEYBOARD;
    inputs[3].ki.wVk = VK_MENU;
    inputs[3].ki.dwFlags = KEYEVENTF_KEYUP;

    // Отправляем события
    SendInput(4, inputs, sizeof(INPUT));

    std::cout << "Смена раскладки клавиатуры выполнена." << std::endl;
}

int main() {
    HHOOK hHook = SetWindowsHookEx(WH_KEYBOARD_LL, KeyboardProc, NULL, 0);
    if (hHook == NULL) {
        std::cerr << "Не удалось установить хук!" << std::endl;
        return 1;
    }

    MSG msg;
    while (GetMessage(&msg, NULL, 0, 0)) {
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }

    UnhookWindowsHookEx(hHook);
    return 0;
}
