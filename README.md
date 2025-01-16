#include <iostream>
using namespace std;

struct Node {
    int value;
    Node* left;
    Node* right;

    Node(int val) : value(val), left(nullptr), right(nullptr) {}
};

// Вспомогательная функция для нахождения минимального значения в дереве
Node* findMin(Node* root) {
    while (root->left != nullptr) {
        root = root->left;
    }
    return root;
}

// Функция удаления узла
Node* deleteNode(Node* root, int key) {
    if (root == nullptr) {
        return root; // Если дерево пустое
    }

    // Поиск узла
    if (key < root->value) {
        root->left = deleteNode(root->left, key);
    } else if (key > root->value) {
        root->right = deleteNode(root->right, key);
    } else {
        // Узел найден
        // Случай 1: Узел не имеет детей
        if (root->left == nullptr && root->right == nullptr) {
            delete root;
            return nullptr;
        }
        // Случай 2: Узел имеет одного ребенка
        if (root->left == nullptr) {
            Node* temp = root->right;
            delete root;
            return temp;
        }
        if (root->right == nullptr) {
            Node* temp = root->left;
            delete root;
            return temp;
        }
        // Случай 3: Узел имеет двух детей
        Node* temp = findMin(root->right); // Минимальный узел в правом поддереве
        root->value = temp->value;        // Замена значения
        root->right = deleteNode(root->right, temp->value); // Удаление дубликата
    }

    return root;
}

// Вспомогательная функция для вставки узла (для тестирования)
Node* insertNode(Node* root, int key) {
    if (root == nullptr) {
        return new Node(key);
    }
    if (key < root->value) {
        root->left = insertNode(root->left, key);
    } else {
        root->right = insertNode(root->right, key);
    }
    return root;
}

// Обход дерева (in-order) для проверки результата
void inorderTraversal(Node* root) {
    if (root != nullptr) {
        inorderTraversal(root->left);
        cout << root->value << " ";
        inorderTraversal(root->right);
    }
}

int main() {
    Node* root = nullptr;
    root = insertNode(root, 50);
    root = insertNode(root, 30);
    root = insertNode(root, 70);
    root = insertNode(root, 20);
    root = insertNode(root, 40);
    root = insertNode(root, 60);
    root = insertNode(root, 80);

    cout << "Tree before deletion: ";
    inorderTraversal(root);
    cout << endl;

    root = deleteNode(root, 50);

    cout << "Tree after deletion: ";
    inorderTraversal(root);
    cout << endl;

    return 0;
}
