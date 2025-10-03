<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "134d8759f0e2ab886e9aa4f62362c201",
  "translation_date": "2025-10-03T12:42:33+00:00",
  "source_file": "TROUBLESHOOTING.md",
  "language_code": "ne"
}
-->
# समस्या समाधान मार्गदर्शिका

यो मार्गदर्शिकाले तपाईंलाई Machine Learning for Beginners पाठ्यक्रमसँग काम गर्दा देखिने सामान्य समस्याहरू समाधान गर्न मद्दत गर्दछ। यदि तपाईंलाई यहाँ समाधान भेटिएन भने, कृपया हाम्रो [Discord Discussions](https://aka.ms/foundry/discord) हेर्नुहोस् वा [नयाँ समस्या रिपोर्ट गर्नुहोस्](https://github.com/microsoft/ML-For-Beginners/issues)।

## सामग्री सूची

- [इन्स्टलेसन समस्याहरू](../..)
- [जुपिटर नोटबुक समस्याहरू](../..)
- [पाइथन प्याकेज समस्याहरू](../..)
- [R वातावरण समस्याहरू](../..)
- [क्विज एप्लिकेसन समस्याहरू](../..)
- [डाटा र फाइल पथ समस्याहरू](../..)
- [सामान्य त्रुटि सन्देशहरू](../..)
- [प्रदर्शन समस्याहरू](../..)
- [वातावरण र कन्फिगरेसन](../..)

---

## इन्स्टलेसन समस्याहरू

### पाइथन इन्स्टलेसन

**समस्या**: `python: command not found`

**समाधान**:
1. [python.org](https://www.python.org/downloads/) बाट Python 3.8 वा उच्च संस्करण इन्स्टल गर्नुहोस्।
2. इन्स्टलेसन जाँच गर्नुहोस्: `python --version` वा `python3 --version`
3. macOS/Linux मा, तपाईंले `python3` प्रयोग गर्नुपर्ने हुन सक्छ।

**समस्या**: धेरै Python संस्करणहरूले द्वन्द्व सिर्जना गर्नु

**समाधान**:
```bash
# Use virtual environments to isolate projects
python -m venv ml-env

# Activate virtual environment
# On Windows:
ml-env\Scripts\activate
# On macOS/Linux:
source ml-env/bin/activate
```

### जुपिटर इन्स्टलेसन

**समस्या**: `jupyter: command not found`

**समाधान**:
```bash
# Install Jupyter
pip install jupyter

# Or with pip3
pip3 install jupyter

# Verify installation
jupyter --version
```

**समस्या**: जुपिटर ब्राउजरमा खुल्दैन

**समाधान**:
```bash
# Try specifying the browser
jupyter notebook --browser=chrome

# Or copy the URL with token from terminal and paste in browser manually
# Look for: http://localhost:8888/?token=...
```

### R इन्स्टलेसन

**समस्या**: R प्याकेजहरू इन्स्टल हुँदैनन्

**समाधान**:
```r
# Ensure you have the latest R version
# Install packages with dependencies
install.packages(c("tidyverse", "tidymodels", "caret"), dependencies = TRUE)

# If compilation fails, try installing binary versions
install.packages("package-name", type = "binary")
```

**समस्या**: IRkernel जुपिटरमा उपलब्ध छैन

**समाधान**:
```r
# In R console
install.packages('IRkernel')
IRkernel::installspec(user = TRUE)
```

---

## जुपिटर नोटबुक समस्याहरू

### कर्नेल समस्याहरू

**समस्या**: कर्नेल बारम्बार बन्द हुन्छ वा पुनः सुरु हुन्छ

**समाधान**:
1. कर्नेल पुनः सुरु गर्नुहोस्: `Kernel → Restart`
2. आउटपुट खाली गरेर पुनः सुरु गर्नुहोस्: `Kernel → Restart & Clear Output`
3. मेमोरी समस्याहरू जाँच गर्नुहोस् ([प्रदर्शन समस्याहरू](../..) हेर्नुहोस्)
4. समस्या भएको कोड पत्ता लगाउन कोशिस गर्नुहोस्।

**समस्या**: गलत Python कर्नेल चयन गरिएको छ

**समाधान**:
1. हालको कर्नेल जाँच गर्नुहोस्: `Kernel → Change Kernel`
2. सही Python संस्करण चयन गर्नुहोस्।
3. यदि कर्नेल हराइरहेको छ भने, यसलाई सिर्जना गर्नुहोस्:
```bash
python -m ipykernel install --user --name=ml-env
```

**समस्या**: कर्नेल सुरु हुँदैन

**समाधान**:
```bash
# Reinstall ipykernel
pip uninstall ipykernel
pip install ipykernel

# Register the kernel again
python -m ipykernel install --user
```

### नोटबुक सेल समस्याहरू

**समस्या**: सेलहरू चलिरहेको छ तर आउटपुट देखाउँदैन

**समाधान**:
1. जाँच गर्नुहोस् कि सेल अझै चलिरहेको छ कि छैन (`[*]` संकेतक हेर्नुहोस्)।
2. कर्नेल पुनः सुरु गरेर सबै सेलहरू चलाउनुहोस्: `Kernel → Restart & Run All`
3. ब्राउजर कन्सोलमा JavaScript त्रुटिहरू जाँच गर्नुहोस् (F12 थिच्नुहोस्)।

**समस्या**: सेलहरू चलाउन सकिँदैन - "Run" क्लिक गर्दा कुनै प्रतिक्रिया छैन

**समाधान**:
1. जुपिटर सर्भर टर्मिनलमा अझै चलिरहेको छ कि छैन जाँच गर्नुहोस्।
2. ब्राउजर पृष्ठ रिफ्रेस गर्नुहोस्।
3. नोटबुक बन्द गरेर पुनः खोल्नुहोस्।
4. जुपिटर सर्भर पुनः सुरु गर्नुहोस्।

---

## पाइथन प्याकेज समस्याहरू

### इम्पोर्ट त्रुटिहरू

**समस्या**: `ModuleNotFoundError: No module named 'sklearn'`

**समाधान**:
```bash
pip install scikit-learn

# Common ML packages for this course
pip install scikit-learn pandas numpy matplotlib seaborn
```

**समस्या**: `ImportError: cannot import name 'X' from 'sklearn'`

**समाधान**:
```bash
# Update scikit-learn to latest version
pip install --upgrade scikit-learn

# Check version
python -c "import sklearn; print(sklearn.__version__)"
```

### संस्करण द्वन्द्व

**समस्या**: प्याकेज संस्करण असंगतता त्रुटिहरू

**समाधान**:
```bash
# Create a new virtual environment
python -m venv fresh-env
source fresh-env/bin/activate  # or fresh-env\Scripts\activate on Windows

# Install packages fresh
pip install jupyter scikit-learn pandas numpy matplotlib seaborn

# If specific version needed
pip install scikit-learn==1.3.0
```

**समस्या**: `pip install` अनुमति त्रुटिहरूका कारण असफल हुन्छ

**समाधान**:
```bash
# Install for current user only
pip install --user package-name

# Or use virtual environment (recommended)
python -m venv venv
source venv/bin/activate
pip install package-name
```

### डाटा लोड गर्ने समस्याहरू

**समस्या**: CSV फाइलहरू लोड गर्दा `FileNotFoundError`

**समाधान**:
```python
import os
# Check current working directory
print(os.getcwd())

# Use relative paths from notebook location
df = pd.read_csv('../../data/filename.csv')

# Or use absolute paths
df = pd.read_csv('/full/path/to/data/filename.csv')
```

---

## R वातावरण समस्याहरू

### प्याकेज इन्स्टलेसन

**समस्या**: प्याकेज इन्स्टलेसन कम्पाइल त्रुटिहरूसँग असफल हुन्छ

**समाधान**:
```r
# Install binary version (Windows/macOS)
install.packages("package-name", type = "binary")

# Update R to latest version if packages require it
# Check R version
R.version.string

# Install system dependencies (Linux)
# For Ubuntu/Debian, in terminal:
# sudo apt-get install r-base-dev
```

**समस्या**: `tidyverse` इन्स्टल हुँदैन

**समाधान**:
```r
# Install dependencies first
install.packages(c("rlang", "vctrs", "pillar"))

# Then install tidyverse
install.packages("tidyverse")

# Or install components individually
install.packages(c("dplyr", "ggplot2", "tidyr", "readr"))
```

### RMarkdown समस्याहरू

**समस्या**: RMarkdown रेन्डर हुँदैन

**समाधान**:
```r
# Install/update rmarkdown
install.packages("rmarkdown")

# Install pandoc if needed
install.packages("pandoc")

# For PDF output, install tinytex
install.packages("tinytex")
tinytex::install_tinytex()
```

---

## क्विज एप्लिकेसन समस्याहरू

### निर्माण र इन्स्टलेसन

**समस्या**: `npm install` असफल हुन्छ

**समाधान**:
```bash
# Clear npm cache
npm cache clean --force

# Remove node_modules and package-lock.json
rm -rf node_modules package-lock.json

# Reinstall
npm install

# If still fails, try with legacy peer deps
npm install --legacy-peer-deps
```

**समस्या**: पोर्ट 8080 पहिले नै प्रयोगमा छ

**समाधान**:
```bash
# Use different port
npm run serve -- --port 8081

# Or find and kill process using port 8080
# On Linux/macOS:
lsof -ti:8080 | xargs kill -9

# On Windows:
netstat -ano | findstr :8080
taskkill /PID <PID> /F
```

### निर्माण त्रुटिहरू

**समस्या**: `npm run build` असफल हुन्छ

**समाधान**:
```bash
# Check Node.js version (should be 14+)
node --version

# Update Node.js if needed
# Then clean install
rm -rf node_modules package-lock.json
npm install
npm run build
```

**समस्या**: लिन्टिङ त्रुटिहरूले निर्माण रोक्छ

**समाधान**:
```bash
# Fix auto-fixable issues
npm run lint -- --fix

# Or temporarily disable linting in build
# (not recommended for production)
```

---

## डाटा र फाइल पथ समस्याहरू

### पथ समस्याहरू

**समस्या**: नोटबुक चलाउँदा डाटा फाइलहरू फेला पर्दैनन्

**समाधान**:
1. **सधैं नोटबुकहरू तिनीहरूको समावेश गर्ने डाइरेक्टरीबाट चलाउनुहोस्।**
   ```bash
   cd /path/to/lesson/folder
   jupyter notebook
   ```

2. **कोडमा सापेक्ष पथहरू जाँच गर्नुहोस्।**
   ```python
   # Correct path from notebook location
   df = pd.read_csv('../data/filename.csv')
   
   # Not from your terminal location
   ```

3. **आवश्यक भएमा पूर्ण पथहरू प्रयोग गर्नुहोस्।**
   ```python
   import os
   base_path = os.path.dirname(os.path.abspath(__file__))
   data_path = os.path.join(base_path, 'data', 'filename.csv')
   ```

### हराएका डाटा फाइलहरू

**समस्या**: डाटासेट फाइलहरू हराइरहेका छन्

**समाधान**:
1. जाँच गर्नुहोस् कि डाटा रिपोजिटरीमा हुनुपर्ने हो - अधिकांश डाटासेटहरू समावेश गरिएका छन्।
2. केही पाठहरूमा डाटा डाउनलोड गर्न आवश्यक हुन सक्छ - पाठको README जाँच गर्नुहोस्।
3. सुनिश्चित गर्नुहोस् कि तपाईंले पछिल्लो परिवर्तनहरू खिच्नुभएको छ:
   ```bash
   git pull origin main
   ```

---

## सामान्य त्रुटि सन्देशहरू

### मेमोरी त्रुटिहरू

**त्रुटि**: `MemoryError` वा डाटा प्रशोधन गर्दा कर्नेल बन्द हुन्छ

**समाधान**:
```python
# Load data in chunks
for chunk in pd.read_csv('large_file.csv', chunksize=10000):
    process(chunk)

# Or read only needed columns
df = pd.read_csv('file.csv', usecols=['col1', 'col2'])

# Free memory when done
del large_dataframe
import gc
gc.collect()
```

### कनभर्जेन्स चेतावनीहरू

**चेतावनी**: `ConvergenceWarning: Maximum number of iterations reached`

**समाधान**:
```python
from sklearn.linear_model import LogisticRegression

# Increase max iterations
model = LogisticRegression(max_iter=1000)

# Or scale your features first
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)
```

### प्लटिङ समस्याहरू

**समस्या**: जुपिटरमा प्लटहरू देखिँदैनन्

**समाधान**:
```python
# Enable inline plotting
%matplotlib inline

# Import pyplot
import matplotlib.pyplot as plt

# Show plot explicitly
plt.plot(data)
plt.show()
```

**समस्या**: Seaborn प्लटहरू फरक देखिन्छन् वा त्रुटि दिन्छन्

**समाधान**:
```python
import warnings
warnings.filterwarnings('ignore', category=UserWarning)

# Update to compatible version
# pip install --upgrade seaborn matplotlib
```

### युनिकोड/एन्कोडिङ त्रुटिहरू

**समस्या**: फाइलहरू पढ्दा `UnicodeDecodeError`

**समाधान**:
```python
# Specify encoding explicitly
df = pd.read_csv('file.csv', encoding='utf-8')

# Or try different encoding
df = pd.read_csv('file.csv', encoding='latin-1')

# For errors='ignore' to skip problematic characters
df = pd.read_csv('file.csv', encoding='utf-8', errors='ignore')
```

---

## प्रदर्शन समस्याहरू

### सुस्त नोटबुक कार्यान्वयन

**समस्या**: नोटबुकहरू चलाउन धेरै सुस्त छन्

**समाधान**:
1. **मेमोरी खाली गर्न कर्नेल पुनः सुरु गर्नुहोस्**: `Kernel → Restart`
2. **प्रयोग नगरिएका नोटबुकहरू बन्द गर्नुहोस्**।
3. **परीक्षणका लागि साना डाटा नमूनाहरू प्रयोग गर्नुहोस्**:
   ```python
   # Work with subset during development
   df_sample = df.sample(n=1000)
   ```
4. **तपाईंको कोड प्रोफाइल गर्नुहोस्** र समस्या क्षेत्र पत्ता लगाउनुहोस्:
   ```python
   %time operation()  # Time single operation
   %timeit operation()  # Time with multiple runs
   ```

### उच्च मेमोरी प्रयोग

**समस्या**: प्रणालीमा मेमोरी सकिन्छ

**समाधान**:
```python
# Check memory usage
df.info(memory_usage='deep')

# Optimize data types
df['column'] = df['column'].astype('int32')  # Instead of int64

# Drop unnecessary columns
df = df[['col1', 'col2']]  # Keep only needed columns

# Process in batches
for batch in np.array_split(df, 10):
    process(batch)
```

---

## वातावरण र कन्फिगरेसन

### भर्चुअल वातावरण समस्याहरू

**समस्या**: भर्चुअल वातावरण सक्रिय हुँदैन

**समाधान**:
```bash
# Windows
python -m venv venv
venv\Scripts\activate.bat

# macOS/Linux
python3 -m venv venv
source venv/bin/activate

# Check if activated (should show venv name in prompt)
which python  # Should point to venv python
```

**समस्या**: प्याकेजहरू इन्स्टल गरिएका छन् तर नोटबुकमा फेला पर्दैनन्

**समाधान**:
```bash
# Ensure notebook uses the correct kernel
# Install ipykernel in your venv
pip install ipykernel
python -m ipykernel install --user --name=ml-env --display-name="Python (ml-env)"

# In Jupyter: Kernel → Change Kernel → Python (ml-env)
```

### Git समस्याहरू

**समस्या**: पछिल्लो परिवर्तनहरू खिच्न सकिँदैन - मर्ज द्वन्द्व

**समाधान**:
```bash
# Stash your changes
git stash

# Pull latest
git pull origin main

# Reapply your changes
git stash pop

# If conflicts, resolve manually or:
git checkout --theirs path/to/file  # Take remote version
git checkout --ours path/to/file    # Keep your version
```

### VS Code एकीकरण

**समस्या**: जुपिटर नोटबुकहरू VS Code मा खुल्दैनन्

**समाधान**:
1. VS Code मा Python एक्सटेन्सन इन्स्टल गर्नुहोस्।
2. VS Code मा Jupyter एक्सटेन्सन इन्स्टल गर्नुहोस्।
3. सही Python इन्टरप्रिटर चयन गर्नुहोस्: `Ctrl+Shift+P` → "Python: Select Interpreter"
4. VS Code पुनः सुरु गर्नुहोस्।

---

## थप स्रोतहरू

- **Discord Discussions**: [#ml-for-beginners च्यानलमा प्रश्न सोध्नुहोस् र समाधानहरू साझा गर्नुहोस्](https://aka.ms/foundry/discord)
- **Microsoft Learn**: [ML for Beginners मोड्युलहरू](https://learn.microsoft.com/en-us/collections/qrqzamz1nn2wx3?WT.mc_id=academic-77952-bethanycheum)
- **भिडियो ट्युटोरियलहरू**: [YouTube प्लेलिस्ट](https://aka.ms/ml-beginners-videos)
- **इश्यू ट्र्याकर**: [बग रिपोर्ट गर्नुहोस्](https://github.com/microsoft/ML-For-Beginners/issues)

---

## अझै समस्या छ?

यदि तपाईंले माथिका समाधानहरू प्रयास गर्नुभयो र अझै समस्या अनुभव गर्दै हुनुहुन्छ भने:

1. **अस्तित्वमा रहेका समस्याहरू खोज्नुहोस्**: [GitHub Issues](https://github.com/microsoft/ML-For-Beginners/issues)
2. **Discord मा छलफलहरू जाँच गर्नुहोस्**: [Discord Discussions](https://aka.ms/foundry/discord)
3. **नयाँ समस्या खोल्नुहोस्**: समावेश गर्नुहोस्:
   - तपाईंको अपरेटिङ सिस्टम र संस्करण
   - Python/R संस्करण
   - त्रुटि सन्देश (पूर्ण ट्रेसब्याक)
   - समस्या पुनः उत्पादन गर्नका लागि चरणहरू
   - तपाईंले पहिले नै प्रयास गर्नुभएको कुरा

हामी यहाँ सहयोग गर्न तयार छौं! 🚀

---

**अस्वीकरण**:  
यो दस्तावेज़ AI अनुवाद सेवा [Co-op Translator](https://github.com/Azure/co-op-translator) प्रयोग गरेर अनुवाद गरिएको हो। हामी शुद्धताको लागि प्रयास गर्छौं, तर कृपया ध्यान दिनुहोस् कि स्वचालित अनुवादमा त्रुटिहरू वा अशुद्धताहरू हुन सक्छ। मूल दस्तावेज़ यसको मातृभाषामा आधिकारिक स्रोत मानिनुपर्छ। महत्वपूर्ण जानकारीको लागि, व्यावसायिक मानव अनुवाद सिफारिस गरिन्छ। यस अनुवादको प्रयोगबाट उत्पन्न हुने कुनै पनि गलतफहमी वा गलत व्याख्याको लागि हामी जिम्मेवार हुनेछैनौं।