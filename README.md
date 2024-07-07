# Study Plan Management System

## Overview

This project is a web-based file management system designed for teachers to upload user information via CSV files. The system generates study plans for each parent based on the uploaded data. Additionally, it includes a file management system allowing administrators to update source files, such as student templates for grades 1-6 (with levels from 111 to 333).

## Features

- **Upload CSV Files**: Teachers can upload CSV files containing user information.
- **Generate Study Plans**: The system processes the CSV file and generates personalized study plans for each parent.
- **File Management**: Administrators can upload, view, and delete template files in the source directory.
- **Multi-level Handling**: Handles different student performance levels (e.g., 111, 222, 333).

## Prerequisites

- Node.js (version 12 or higher)
- npm (Node Package Manager)

## Installation

### Windows

1. **Install Node.js**:

    Download and install Node.js from [nodejs.org](https://nodejs.org/).

2. **Clone the repository**:

    Open Command Prompt and run:

    ```sh
    git clone https://github.com/yourusername/file-management-system.git
    cd file-management-system
    ```

3. **Install dependencies**:

    ```sh
    npm install
    ```

### macOS

1. **Install Node.js**:

    You can use Homebrew to install Node.js. If you don't have Homebrew installed, you can install it from [brew.sh](https://brew.sh/).

    ```sh
    brew install node
    ```

2. **Clone the repository**:

    Open Terminal and run:

    ```sh
    git clone https://github.com/yourusername/file-management-system.git
    cd file-management-system
    ```

3. **Install dependencies**:

    ```sh
    npm install
    ```

### Linux

1. **Install Node.js**:

    On Debian-based distributions (e.g., Ubuntu), you can use:

    ```sh
    sudo apt update
    sudo apt install nodejs npm
    ```

    On RHEL-based distributions (e.g., CentOS), you can use:

    ```sh
    sudo yum install nodejs npm
    ```

2. **Clone the repository**:

    Open Terminal and run:

    ```sh
    git clone https://github.com/yourusername/file-management-system.git
    cd file-management-system
    ```

3. **Install dependencies**:

    ```sh
    npm install
    ```

## Usage

### Start the Server

In the project directory, run the following command:

```sh
node server.js
```

### Access the Web Interface

Open your web browser and navigate to `http://localhost:3000/model.html`.

## Project Structure

```plaintext
file-management-system/
│
├── public/
│   ├── css/
│   │   └── style.css
│   ├── index.html
│   ├── create.html
│   ├── instruction.html
│   ├── server.html
│   └── model.html
│
├── uploads/
│   └── (uploaded files)
│
├── source/
│   └── (source files for templates)
│
├── processed/
│   └── (processed files for download)
│
├── server.js
├── package.json
└── README.md
```

## Modifying Templates

### Updating Templates

To update the templates, replace the existing files in the `source` directory with the new templates. The templates should follow the naming convention required by the specific logic in the `server.js` file.

### Code Changes

If the template filenames or logic for selecting templates change, you need to update the `processFile` function in `server.js`.

#### Example: Grade 1-6 with Levels 111-333

Here is an example of how to handle grades 1-6 with levels:

```javascript
const processFile = (filePath, processedFolder, callback) => {
    const results = [];
    fs.createReadStream(filePath)
        .pipe(csv())
        .on('data', (data) => results.push(data))
        .on('end', () => {
            results.forEach(row => {
                const grade = row['目前年级'];
                const wechatId = row['您的微信昵称（方便发您计划）'];
                let code = '';
                let sourceFile = '';

                if (['1年级', '2年级', '3年级', '4年级', '5年级', '6年级'].includes(grade)) {
                    const chineseScore = row['语文成绩'];
                    const mathScore = row['数学成绩'];
                    const englishScore = row['英语成绩'];

                    if (chineseScore === '96以上') {
                        code += '1';
                    } else if (chineseScore === '90-95') {
                        code += '2';
                    } else {
                        code += '3';
                    }

                    if (mathScore === '96以上') {
                        code += '1';
                    } else if (mathScore === '90-95') {
                        code += '2';
                    } else {
                        code += '3';
                    }

                    if (englishScore === '96以上') {
                        code += '1';
                    } else if (englishScore === '90-95') {
                        code += '2';
                    } else {
                        code += '3';
                    }

                    sourceFile = `${code}${grade[0]}年级学习计划done.xlsx`;
                } else {
                    sourceFile = `${grade}暑期学习计划.xlsx`;
                }

                const sourcePath = path.join(__dirname, 'source', sourceFile);
                const targetFile = `${grade}暑期学习计划-${wechatId}.xlsx`;
                const targetPath = path.join(processedFolder, targetFile);

                try {
                    fs.copyFileSync(sourcePath, targetPath);
                    console.log(`File copied: ${targetPath}`);
                } catch (err) {
                    console.error(`Error while copying ${sourcePath} to ${targetPath}:`, err);
                }
            });
            callback();
        });
};
```

### Example: Middle School with Levels

If middle school grades (e.g., 初一, 初二, 初三) also require level-specific templates, update the `processFile` function accordingly:

```javascript
const processFile = (filePath, processedFolder, callback) => {
    const results = [];
    fs.createReadStream(filePath)
        .pipe(csv())
        .on('data', (data) => results.push(data))
        .on('end', () => {
            results.forEach(row => {
                const grade = row['目前年级'];
                const wechatId = row['您的微信昵称（方便发您计划）'];
                let code = '';
                let sourceFile = '';

                if (['初一', '初二', '初三'].includes(grade)) {
                    const chineseScore = row['语文成绩'];
                    const mathScore = row['数学成绩'];
                    const englishScore = row['英语成绩'];

                    if (chineseScore === '96以上') {
                        code += '1';
                    } else if (chineseScore === '90-95') {
                        code += '2';
                    } else {
                        code += '3';
                    }

                    if (mathScore === '96以上') {
                        code += '1';
                    } else if (mathScore === '90-95') {
                        code += '2';
                    } else {
                        code += '3';
                    }

                    if (englishScore === '96以上') {
                        code += '1';
                    } else if (englishScore === '90-95') {
                        code += '2';
                    } else {
                        code += '3';
                    }

                    sourceFile = `${code}${grade}学习计划done.xlsx`;
                } else {
                    sourceFile = `${grade}暑期学习计划.xlsx`;
                }

                const sourcePath = path.join(__dirname, 'source', sourceFile);
                const targetFile = `${grade}暑期学习计划-${wechatId}.xlsx`;
                const targetPath = path.join(processedFolder, targetFile);

                try {
                    fs.copyFileSync(sourcePath, targetPath);
                    console.log(`File copied: ${targetPath}`);
                } catch (err) {
                    console.error(`Error while copying ${sourcePath} to ${targetPath}:`, err);
                }
            });
            callback();
        });
};
```

### Uploading New Templates

1. **Navigate to the File Management Page**:

    Open your web browser and go to `http://localhost:3000/model.html`.

2. **Upload New Templates**:

    Use the file upload interface to upload new template files. Ensure the filenames follow the required naming conventions.

3. **Update the Code**:

    If the logic for selecting templates has changed, update the `processFile` function in `server.js`.

4. **Restart the Server**:

    After making changes to `server.js`, restart the server:

    ```sh
    node server.js
    ```

## Contributing

1. Fork the repository.
2. Create your feature branch (`git checkout -b feature/my-new-feature`).
3. Commit your changes (`git commit -am 'Add some feature'`).
4. Push to the branch (`git push origin feature/my-new-feature`).
5. Create a new Pull Request.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Acknowledgements

- Icons by [Material Symbols](https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@20..48,100..700,0..1,-50..200).
- Normalize.css by [necolas](https://necolas.github.io/normalize.css/).
