# rotate_2

    // this contains header, implementation and main files for rotating a linked list of integers
    // using templates
    // C++
    
    // Linnea Scholin

    #ifndef H_LinkedListType
    #define H_LinkedListType

    #include <iostream>
    using namespace std;



    template <class Type>
    struct nodeType
    {
	   Type info;
	   nodeType<Type> *link;
    };

    template <class Type>
    class linkedListIterator
    {
    public:
    linkedListIterator();
    linkedListIterator(nodeType<Type> *ptr);
    Type operator*();
    linkedListIterator<Type> operator++();
    bool operator==(const linkedListIterator<Type>& right) const;
    bool operator!=(const linkedListIterator<Type>& right) const;

    private:
    nodeType<Type> *current;
    };

    template <class Type>
    linkedListIterator<Type>::linkedListIterator()
    {
    current = NULL;
    }

    template <class Type>
    linkedListIterator<Type>::linkedListIterator(nodeType<Type> *ptr)
    {
    current = ptr;
    }

    template <class Type>
    Type linkedListIterator<Type>::operator*()
    {
    return current->info;
    }

    template <class Type>
    linkedListIterator<Type> linkedListIterator<Type>::operator++()
    {
    current = current->link;

    return *this;
    }

    template <class Type>
    bool linkedListIterator<Type>::operator==(const linkedListIterator<Type>& right) const
    {
    return (current == right.current);
    }

    template <class Type>
    bool linkedListIterator<Type>::operator!=(const linkedListIterator<Type>& right) const
    {
    return (current != right.current);
    }

    template <class Type>
    class linkedListType
    {
    public:
      const linkedListType<Type>& operator=(const linkedListType<Type>&);
      void initializeList();
      bool isEmptyList() const;
      void print() const;
      int length() const;
      void destroyList();
      void insert(const Type& newItem);
      void rotate();
      Type front() const;
      Type back() const;
      bool search(const Type& searchItem) const;
      linkedListIterator<Type> begin();
      linkedListIterator<Type> end();
      linkedListType();
      linkedListType(const linkedListType<Type>& otherList);
      ~linkedListType();

    private:
      int count;
      nodeType<Type> *first;
      nodeType<Type> *last;
      void copyList(const linkedListType<Type>& otherList);
    };

    template <class Type>
    const linkedListType<Type>& linkedListType<Type>::operator=(const linkedListType<Type>& otherList)
    {
      if (this != &otherList)
      {
        copyList(otherList);
      }

    return *this;
    }

    template <class Type>
    void linkedListType<Type>::initializeList()
    {
	    destroyList();
    }

    template <class Type>
    bool linkedListType<Type>::isEmptyList() const
    {
    return (first == NULL);
    }

    template <class Type>
    void linkedListType<Type>::print() const
    {
    nodeType<Type> *current;

    current = first;

    while (current != NULL)
    {
        cout << current->info << " ";
        current = current->link;
    }
    cout << endl;
    }

    template <class Type>
    int linkedListType<Type>::length() const
    {
    return count;
    }

    template <class Type>
    void linkedListType<Type>::destroyList()
    {
    nodeType<Type> *temp;
    while (first != NULL)
    {
        temp = first;
        first = first->link;
        delete temp;
    }

    last = NULL;

    count = 0;
    }

    template<class Type>
    void linkedListType<Type>::insert(const Type& newItem)
    {
     nodeType<Type> *current;
    nodeType<Type> *trailCurrent;
    nodeType<Type> *newNode;

    bool  found;

    newNode = new nodeType<Type>;
    newNode->info = newItem;
    newNode->link = NULL;


    if (first == NULL)
    {
        first = newNode;
        last = newNode;
        count++;
    }
    else
    {
        current = first;
        found = false;

        while (current != NULL && !found)
           if (current->info >= newItem)
               found = true;
           else
           {
               trailCurrent = current;
               current = current->link;
           }

        if (current == first)
        {
            newNode->link = first;
            first = newNode;
            count++;
        }
        else
        {
            trailCurrent->link = newNode;
            newNode->link = current;

            if (current == NULL)
                last = newNode;

            count++;
        }
    }
    }

    template<class Type>
    void linkedListType<Type> :: rotate()
    {
    nodeType<Type> *newNode, *p;
    newNode = new nodeType<Type>;

    newNode->info = first->info;
    newNode->link = NULL;
    first = first->link;
    last->link = newNode;
    }

    template <class Type>
    Type linkedListType<Type>::front() const
    {
    assert(first != NULL);

    return first->info;
    }

    template <class Type>
    Type linkedListType<Type>::back() const
    {
    assert(last != NULL);

    return last->info;
    }

    template<class Type>
    bool linkedListType<Type>::search(const Type& searchItem) const
    {
    nodeType<Type> *current;
    bool found = false;

    current = first;

    while(current!= NULL && !found)
        if (current->info == searchItem)
            found = true;
        else
            current = current->link;

    return found;
    }

    template <class Type>
    linkedListIterator<Type> linkedListType<Type>::begin()
    {
    linkedListIterator<Type> temp(first);

    return temp;
    }

    template <class Type>
    linkedListIterator<Type> linkedListType<Type>::end()
    {
    linkedListIterator<Type> temp(NULL);

    return temp;
    }

    template <class Type>
    linkedListType<Type>::linkedListType()
    {
      first = NULL;
    last = NULL;
    count = 0;
    }

    template <class Type>
    linkedListType<Type>::linkedListType(const linkedListType<Type>& otherList)
    {
   	first = NULL;
    copyList(otherList);
    }

    template <class Type>
    linkedListType<Type>::~linkedListType()
    {
    destroyList();
    }

    template <class Type>
    void linkedListType<Type>::copyList(const linkedListType<Type>& otherList)
    {
    nodeType<Type> *newNode;
    nodeType<Type> *current;

    if (first != NULL)
       destroyList();

    if (otherList.first == NULL)
    {
        first = NULL;
        last = NULL;
        count = 0;
    }
    else
    {
        current = otherList.first;

        count = otherList.count;

        first = new nodeType<Type>;

        first->info = current->info;
        first->link = NULL;

        last = first;

        current = current->link;

        while (current != NULL)
        {
            newNode = new nodeType<Type>;
            newNode->info = current->info;
            newNode->link = NULL;

            last->link = newNode;
            last = newNode;

            current = current->link;
        }
    }
    }

    #endif

// main program to test function rotate

    int main()
    {
    linkedListType<int> list1;
    int num;

    cout << "Enter a list of integers ending with -999: ";
    cin >> num;

    while (num != -999)
    {
        list1.insert(num);
        cin >> num;
    }

    cout << endl;

    cout << "The list entered is: ";
    list1.print();

    list1.rotate();

    cout << endl;

    cout << "After rotating the list, it is now: ";
    list1.print();


    return 0;
    }
